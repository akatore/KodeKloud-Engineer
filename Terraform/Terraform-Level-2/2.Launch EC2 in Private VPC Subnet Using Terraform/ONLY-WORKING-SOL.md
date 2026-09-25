The Nautilus DevOps team is expanding their AWS infrastructure and requires the setup of a private Virtual Private Cloud (VPC) along with a subnet. This VPC and subnet configuration will ensure that resources deployed within them remain isolated from external networks and can only communicate within the VPC. Additionally, the team needs to provision an EC2 instance under the newly created private VPC. This instance should be accessible only from within the VPC, allowing for secure communication and resource management within the AWS environment.

1. Create a VPC named `nautilus-priv-vpc` with the CIDR block `10.0.0.0/16`.
2. Create a subnet named `nautilus-priv-subnet` inside the VPC with the CIDR block `10.0.1.0/24` and `auto-assign` IP option must not be `enabled`.
3. Create an EC2 instance named `nautilus-priv-ec2` inside the subnet and instance type must be `t2.micro`.
4. Ensure the security group of the EC2 instance allows access only from within the VPC's CIDR block.
5. Create the `main.tf` file (do not create a separate `.tf` file) to provision the VPC, subnet and EC2 instance.
6. Use `variables.tf` file with the following variable names:
    - `KKE_VPC_CIDR` for the VPC CIDR block.
    - `KKE_SUBNET_CIDR` for the subnet CIDR block.
7. Use the `outputs.tf` file with the following variable names:
    - `KKE_vpc_name` for the name of the VPC.
    - `KKE_subnet_name` for the name of the subnet.
    - `KKE_ec2_private` for the name of the EC2 instance.

**Notes:**

1. The Terraform working directory is `/home/bob/terraform`.
2. Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.
3. Before submitting the task, ensure that `terraform plan` returns `No changes. Your infrastructure matches the configuration.`


### `variables.tf`
```
variable "KKE_VPC_CIDR" {
  default = "10.0.0.0/16"
  type = string
}

variable "KKE_SUBNET_CIDR" {
  default = "10.0.1.0/24"
  type = string
}

variable "prefix" {
  default = "nautilus-priv"
  type = string
}
```

### `main.tf`
```
resource "aws_vpc" "vpc" {
    cidr_block = var.KKE_VPC_CIDR
    tags = {
        Name = "${var.prefix}-vpc"
    }
}

resource "aws_subnet" "subnet" {
    vpc_id = aws_vpc.vpc.id
    cidr_block = var.KKE_SUBNET_CIDR
    tags = {
        Name = "${var.prefix}-subnet"
    }

}

resource "aws_security_group" "allow_tls" {
  name        = "allow_tls"
  description = "Allow TLS inbound traffic and all outbound traffic"
  vpc_id      = aws_vpc.vpc.id

  tags = {
    Name = "allow_tls"
  }
}

resource "aws_vpc_security_group_ingress_rule" "allow_https" {
  security_group_id = aws_security_group.allow_tls.id
  cidr_ipv4         = var.KKE_VPC_CIDR
  from_port         = 80
  ip_protocol       = "tcp"
  to_port           = 80
}

resource "aws_vpc_security_group_ingress_rule" "allow_ssh" {
  security_group_id = aws_security_group.allow_tls.id
  cidr_ipv4         = var.KKE_VPC_CIDR
  from_port         = 22
  ip_protocol       = "tcp"
  to_port           = 22
}

resource "aws_vpc_security_group_egress_rule" "allow_all_outbound" {
  security_group_id = aws_security_group.allow_tls.id
  cidr_ipv4         = "0.0.0.0/0"
  ip_protocol       = "-1" # semantically equivalent to all ports

}

data "aws_ami" "amazon_linux" {
  most_recent = true

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-ebs"]
  }

  owners = ["amazon"] # Canonical
}

resource "aws_instance" "ec2" {
    subnet_id = aws_subnet.subnet.id
    ami           = data.aws_ami.amazon_linux.id
    instance_type = "t2.micro"
    vpc_security_group_ids = [aws_security_group.allow_tls.id]
    tags = {
        Name = "${var.prefix}-ec2"
    }
}
```



### `outputs.tf`
```
output "KKE_vpc_name" {
  value = aws_vpc.vpc.tags["Name"]
}

output "KKE_subnet_name" {
  value = aws_subnet.subnet.tags["Name"]
}

output "KKE_ec2_private" {
  value = aws_instance.ec2.tags["Name"]
}
```
## Referrences

- [VPC Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)
- [Subnet](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet)
- [Security Group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group)
- [Ingress Rule](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc_security_group_ingress_rule)
- [Egress Rule](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc_security_group_egress_rule)
- [AMI Data Source](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ami)
- [AWS Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)

## Good To Know

- **Private VPC**: Creates isolated network environment with no internet gateway attached by default
- **CIDR Planning**: /16 VPC allows 65,536 IPs, /24 subnet allows 256 IPs (minus AWS reserved)
- **map_public_ip_on_launch**: Setting to false ensures instances get only private IPs
- **Security Group Scope**: VPC-specific security groups provide more granular control than EC2-Classic
- **Data Sources**: Using AMI data source ensures latest image without hardcoding AMI IDs
- **VPC Security Groups**: Must specify vpc_security_group_ids instead of security_groups for VPC instances
- **Ingress vs Egress**: Ingress controls inbound traffic, egress controls outbound traffic
- **CIDR Restrictions**: Limiting access to VPC CIDR (10.0.0.0/16) ensures internal-only communication
- **Resource Dependencies**: Terraform automatically handles creation order (VPC → Subnet → Security Group → Instance)
- **Private Connectivity**: Instance requires NAT Gateway or VPC Endpoints for internet access
- **AWS Reserved IPs**: First 4 and last IP in each subnet are reserved by AWS
- **Instance Placement**: Subnet determines availability zone and IP range for the instance


<img width="817" height="575" alt="image" src="https://github.com/user-attachments/assets/89b22ff3-d32b-49fb-94fe-9d8cc31587c0" />
