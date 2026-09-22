The Nautilus DevOps team is automating VPC creation using Terraform to manage networking efficiently. As part of this task, they need to create a VPC with specific requirements.

For this task, create an AWS VPC using `Terraform` with the following requirements:

1. The VPC name `devops-vpc` should be stored in a variable named `KKE_vpc`.
2. The VPC should have a CIDR block of `10.0.0.0/16`.

**Note:**

1. The configuration values should be stored in a `variables.tf` file.
2. The Terraform script should be structured with a `main.tf` file referencing `variables.tf`.
3. The Terraform working directory is `/home/bob/terraform`.
4. Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

main.tf
```
resource "aws_vpc" "devops-vpc" {
  cidr_block = var.cidr
  tags = {
    Name = var.KKE_vpc
  }
}
```

variables.tf
```
variable "KKE_vpc" {
  description = "Name to be used on all the resources as identifier"
  type        = string
  default     = "devops-vpc"
}

variable "cidr" {
  description = "(Optional) The IPv4 CIDR block for the VPC. CIDR can be explicitly set or it can be derived from IPAM using `ipv4_netmask_length` & `ipv4_ipam_pool_id`"
  type        = string
  default     = "10.0.0.0/16"
}
```

<img width="752" height="621" alt="image" src="https://github.com/user-attachments/assets/465ec697-f4b5-4c02-ac1b-e600572694cf" />

----

The Nautilus DevOps team is automating VPC creation using Terraform to manage networking efficiently. As part of this task, they need to create a VPC with specific requirements.

For this task, create an AWS VPC using `Terraform` with the following requirements:

1. The VPC name `devops-vpc` should be stored in a variable named `KKE_vpc`.
2. The VPC should have a CIDR block of `10.0.0.0/16`.

**Note:**

1. The configuration values should be stored in a `variables.tf` file.
2. The Terraform script should be structured with a `main.tf` file referencing `variables.tf`.
3. The Terraform working directory is `/home/bob/terraform`.
4. Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

main.tf
```
resource "aws_vpc" "devops-vpc" {
    cidr_block = var.cidr
    tags = {
        Name = var.KKE_vpc
    }
}
```

variables.tf
```
variable "KKE_vpc" {
    type = string
    default = "devops-vpc"
}

variable "cidr" {
    type = string
    default = "10.0.0.0/16"
}

```

<img width="707" height="707" alt="image" src="https://github.com/user-attachments/assets/b7c74cf6-067d-4022-bdae-5d61bc2bd05f" />
