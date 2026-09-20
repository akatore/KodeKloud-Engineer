During the migration process, the Nautilus DevOps team created several EC2 instances in different regions. They are currently in the process of identifying the correct resources and utilization and are making continuous changes to ensure optimal resource utilization. Recently, they discovered that one of the EC2 instances was underutilized, prompting them to decide to change the instance type. Please make sure the `Status check` is completed (if it's still in `Initializing` state) before making any changes to the instance.

1. Change the instance type from `t2.micro` to `t2.nano` for `datacenter-ec2` instance using `terraform`.
2. Make sure the EC2 instance `datacenter-ec2` is in `running` state after the change.
3. The Terraform working directory is `/home/bob/terraform`. Update the `main.tf` file (do not create a separate `.tf` file) to change the instance type.
    
    `Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

```
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.nano"
  subnet_id     = ""
  vpc_security_group_ids = [
    "sg-9214706ba03f84085"
  ]

  tags = {
    Name = "datacenter-ec2"
  }
}

output "ec2_instance_type" {
  value = aws_instance.ec2.instance_type
}

```

![Uploading image.png…]()

----
You see subnet_id = "" (an empty string) because your lab environment is relying on AWS's Default Subnet feature [INDEX].
When you leave the subnet_id blank or pass an empty string, Terraform explicitly instructs the AWS API: "Launch this EC2 instance into whichever default subnet is available in my default VPC."
Here is a quick breakdown of why this happens and how it works under the hood:
## 1. The Default VPC Mechanism
Every AWS account comes pre-configured with a Default VPC out of the box. Inside that Default VPC, AWS automatically creates one Default Subnet in every single Availability Zone (e.g., us-east-1a, us-east-1b, etc.).
Because these exist automatically, AWS doesn't force you to look up and type out a specific subnet ID (like subnet-012345abcde) if you just want a quick sandbox server. Leaving it empty tells AWS to pick one for you.
## 2. Is subnet_id = "" valid in real production?
In a real-world enterprise engineering job, no. Writing subnet_id = "" is generally avoided for two reasons:

* Lack of Control: You won't know which data center building (Availability Zone) your server will land in.
* Security Risks: Default subnets are almost always public subnets (they automatically assign public IPs and route straight to the internet). In production, you want your app servers hidden away in private subnets with strict routing rules.

## 3. How you would write it in a real project
Instead of an empty string, real-world code uses dynamic data tracking to fetch a subnet and explicitly attach it, like this:

# 1. Fetch a specific private subnet dynamically
data "aws_subnet" "private_app_zone" {
  filter {
    name   = "tag:Name"
    values = ["production-private-subnet-1a"]
  }
}

# 2. Feed the tracked ID directly into the instance configuration
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.nano"
  
  # FIXED: Explicitly targeting a secure subnet instead of leaving it empty
  subnet_id     = data.aws_subnet.private_app_zone.id 
}

For your current sandbox lab environment, keeping it as subnet_id = "" is completely fine and expected, as it keeps your code ultra-lean and lightweight!
Did your final terraform apply pass successfully with the t2.nano configuration, and are you ready to tackle the next lab resource block?

