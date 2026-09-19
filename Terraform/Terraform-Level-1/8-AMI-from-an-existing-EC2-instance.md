The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

1. For this task, create an AMI from an existing EC2 instance named `datacenter-ec2` using Terraform.
2. Name of the AMI should be `datacenter-ec2-ami`, make sure AMI is in `available` state.
3. The Terraform working directory is `/home/bob/terraform`. Update the `main.tf` file (do not create a separate `.tf` file) to create the AMI.
    
    `Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.
    

```jsx
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  vpc_security_group_ids = [
    "sg-936a9c61c7ddf15cb"
  ]

  tags = {
    Name = "datacenter-ec2"
  }
}

output "aws_instance_id"  {
  value = aws_instance.ec2.id

}

resource "aws_ami_from_instance" "datacenter-ec2-ami" {
  name               = "datacenter-ec2-ami"
  source_instance_id = aws_instance.ec2.id
}

output "ami_arn" {
  value = aws_ami_from_instance.datacenter-ec2-ami.arn
}

output "ami_id" {
  value = aws_ami_from_instance.datacenter-ec2-ami.id
}

```

<img width="801" height="550" alt="image" src="https://github.com/user-attachments/assets/05824316-5aeb-4065-a32f-85c5ec89e935" />
