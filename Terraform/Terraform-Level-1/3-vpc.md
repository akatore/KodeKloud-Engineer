The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

Create a VPC named `nautilus-vpc` in region `us-east-1` with any `IPv4` CIDR block through terraform.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

```
resource "aws_vpc" "nautilus-vpc" {
    cidr_block = "10.0.0.0/16"

    tags = {
      Name = "nautilus-vpc"
    }
}
}

output "vpc_id" {
  value = aws_vpc.nautilus-vpc.tags["Name"]
}
```
example
```
resource "aws_vpc" "devops-vpc" {
    cidr_block = "10.0.0.0/16"
    tags = {
        Name = "devops-vpc"
    }
}
output "aws_vpc_name" {
    value = aws_vpc.devops-vpc.tags["Name"]
}

```
terraform init
terraform plan
```

![alt text](image-3.png)

`terraform apply`

![alt text](image-4.png)

