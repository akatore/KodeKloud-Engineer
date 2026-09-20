The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.

There is an instance named `datacenter-ec2` and an elastic-ip named `datacenter-ec2-eip` in `us-east-1` region. Attach the `datacenter-ec2-eip` elastic-ip to the `datacenter-ec2` instance using `Terraform` only. The Terraform working directory is `/home/bob/terraform`. Update the `main.tf` file (do not create a separate `.tf` file) to attach the specified Elastic IP to the instance.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

```
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  subnet_id     = "subnet-cd32c5f85d78213da"
  vpc_security_group_ids = [
    "sg-c06d924520065e0a6"
  ]

  tags = {
    Name = "datacenter-ec2"
  }
}

# Provision Elastic IP
resource "aws_eip" "ec2_eip" {
  tags = {
    Name = "datacenter-ec2-eip"
  }
  domain = "vpc"
  # Attach the Elastic IP to your managed EC2 instance bloc
  instance = aws_instance.ec2.id

}
```

https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip
