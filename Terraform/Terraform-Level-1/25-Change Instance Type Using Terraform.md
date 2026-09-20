<img width="782" height="670" alt="image" src="https://github.com/user-attachments/assets/dcfbcb1b-9d75-492b-aef8-00063f286099" />During the migration process, the Nautilus DevOps team created several EC2 instances in different regions. They are currently in the process of identifying the correct resources and utilization and are making continuous changes to ensure optimal resource utilization. Recently, they discovered that one of the EC2 instances was underutilized, prompting them to decide to change the instance type. Please make sure the `Status check` is completed (if it's still in `Initializing` state) before making any changes to the instance.

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

<img width="782" height="670" alt="image" src="https://github.com/user-attachments/assets/c501be31-9d6c-4f51-a6b3-c63f2561d42c" />



----
You see subnet_id = "" (an empty string) because your lab environment is relying on AWS's Default Subnet feature [INDEX].
When you leave the subnet_id blank or pass an empty string, Terraform explicitly instructs the AWS API: "Launch this EC2 instance into whichever default subnet is available in my default VPC."
Here is a quick breakdown of why this happens and how it works under the hood:
### 1. The Default VPC Mechanism
Every AWS account comes pre-configured with a Default VPC out of the box. Inside that Default VPC, AWS automatically creates one Default Subnet in every single Availability Zone (e.g., us-east-1a, us-east-1b, etc.).
Because these exist automatically, AWS doesn't force you to look up and type out a specific subnet ID (like subnet-012345abcde) if you just want a quick sandbox server. Leaving it empty tells AWS to pick one for you.
### 2. Is subnet_id = "" valid in real production?
In a real-world enterprise engineering job, no. Writing subnet_id = "" is generally avoided for two reasons:

* Lack of Control: You won't know which data center building (Availability Zone) your server will land in.
* Security Risks: Default subnets are almost always public subnets (they automatically assign public IPs and route straight to the internet). In production, you want your app servers hidden away in private subnets with strict routing rules.

### 3. How you would write it in a real project
Instead of an empty string, real-world code uses dynamic data tracking to fetch a subnet and explicitly attach it, like this:

## 1. Fetch a specific private subnet dynamically
data "aws_subnet" "private_app_zone" {
  filter {
    name   = "tag:Name"
    values = ["production-private-subnet-1a"]
  }
}

## 2. Feed the tracked ID directly into the instance configuration
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.nano"
  
  # FIXED: Explicitly targeting a secure subnet instead of leaving it empty
  subnet_id     = data.aws_subnet.private_app_zone.id 
}

----

## so if we change the instance_type how its done underneatch the ec2, lets say they had the old apps data

When you update the instance_type in your Terraform file and run terraform apply, Terraform does not delete the server or wipe the hard drive.
Instead, it triggers a safe sequence called an In-Place Update. Underneath the hood, AWS handles this exactly like turning off a desktop computer, swapping out the CPU and RAM sticks, and turning it back on. All of your application data, configurations, and files remain completely safe and untouched.
Here is the exact step-by-step sequence of what happens underneath the hood:
------------------------------
### The Step-by-Step AWS Sequence

[Running: t2.micro] ──► [1. API: StopInstance] ──► [2. Detach Host] 
                                                        │
[Running: t2.nano]  ◄── [4. API: StartInstance] ◄── [3. Change Specs]


   1. Terraform Sends a Stop Command: Terraform calls the AWS EC2 API (StopInstances). The operating system performs a graceful shutdown.
   2. The Server Detaches from the Physical Host: In the AWS data center, your virtual machine is detached from the physical blade server rack that was providing it with t2.micro resources (1 vCPU, 1 GiB RAM).
   3. AWS Modifies the Attributes: The virtual machine profile is updated to the t2.nano specification (1 vCPU, 0.5 GiB RAM).
   4. The Storage Disk Stays Put: The underlying virtual hard drive (the Root EBS Volume holding your operating system, application code, and data) never leaves its storage array. It stays plugged into your instance profile throughout the entire process.
   5. Terraform Sends a Start Command: Terraform calls the AWS EC2 API (StartInstances). AWS finds a new physical blade server rack with available space, attaches your original storage disk to it, and boots the operating system back up.

------------------------------
### What stays the same vs. What changes?

| 🔒 What is SAFELY PRESERVED (No Data Loss) | ⚠️ What MIGHT CHANGE |
|---|---|
| Your Files & Folders: Every file in /var/www, /home, or databases. | Public IP Address: Unless you are using an Elastic IP, a standard public IP will change when an instance stops and starts. |
| Private IP Address: Internal networking addresses stay locked. | RAM Capacity: Available memory drops from 1 GiB to 0.5 GiB. |
| Volume Attachments: Secondary EBS data drives remain connected. | Application Performance: Heavy apps might slow down due to less RAM. |
| Security Groups & IAM Roles: Access rules remain identical. | |

### ⚠️ A Critical Warning for System Engineers
While your data is completely safe on the hard drive, changing an instance from a t2.micro to a t2.nano cuts your available RAM in half (from 1,024 MB down to 512 MB).
If your application needs at least 600 MB of RAM to run, the OS will boot up perfectly fine, but your application will crash with an Out of Memory (OOM) error the moment it tries to start. Always ensure your application can handle the smaller hardware specifications before shrinking an instance type!
To ensure your infrastructure is running optimally, let me know:

* Is your application currently running successfully on the new t2.nano sizing?
* Do you need help checking the server's live RAM usage via the terminal to ensure it isn't running out of memory?




For your current sandbox lab environment, keeping it as subnet_id = "" is completely fine and expected, as it keeps your code ultra-lean and lightweight!
Did your final terraform apply pass successfully with the t2.nano configuration, and are you ready to tackle the next lab resource block?

