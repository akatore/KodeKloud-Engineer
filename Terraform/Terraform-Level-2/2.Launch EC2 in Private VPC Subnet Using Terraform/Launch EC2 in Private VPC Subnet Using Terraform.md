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
  

### `main.tf`
```hcl
resource "aws_vpc" "nautilus-priv" {
    cidr_block = var.KKE_VPC_CIDR 
    tags = {
        Name = var.KKE_vpc_name
    }
}

resource "aws_subnet" "nautilus-priv" {
    vpc_id = aws_vpc.nautilus-priv.id
    cidr_block = var.KKE_SUBNET_CIDR
    tags = {
        Name = var.KKE_subnet_name
    }
    map_public_ip_on_launch = false # Disables auto-assign public IPv4 address

}

resource "aws_instance" "nautilus-priv" {
  ami = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"

  tags = {
    Name = var.KKE_instance_name
  }
}

```


### `variables.tf`

```hcl
variable "KKE_vpc_name" {
    default = "nautilus-priv-vpc"
    type = string
}
variable "KKE_subnet_name" {
    default = "nautilus-priv-subnet"
    type = string
}
variable "KKE_VPC_CIDR" {
    default = "10.0.0.0/16"
    type = string
}
variable "KKE_SUBNET_CIDR" {
    default = "10.0.1.0/24"
    type = string
}

variable "KKE_instance_name" {
    default = "nautilus-priv-ec2"
    type = string
}

```

### `output.tf`

```hcl

output "KKE_subnet_name" {
    value = aws_subnet.nautilus-priv.tags["Name"]
}

output "kke_vpc_name" {
    value = aws_vpc.nautilus-priv.tags["Name"]
}
```
-----

### `main.tf`
```
resource "aws_vpc" "datacenter-priv" {
    cidr_block = var.KKE_VPC_CIDR
    tags = {
        Name = "datacenter-priv-vpc"
    }
}


resource "aws_subnet" "datacenter-priv" {
    cidr_block = var.KKE_SUBNET_CIDR
    vpc_id = aws_vpc.datacenter-priv.id
    tags = {
        Name = "datacenter-priv-subnet"
    }
    map_public_ip_on_launch = false
}

resource "aws_instance" "datacenter-priv" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"

    tags = {
        Name = "datacenter-priv-ec2"
    }
}
```

### `variable.tf`

```
variable "KKE_VPC_CIDR" {
    default = "10.0.0.0/16"
    type = string
}

variable "KKE_SUBNET_CIDR" {
    default = "10.0.1.0/24"
    type = string
}
```

### `outputs.tf`
```
output "KKE_vpc_name" {
    value = aws_vpc.datacenter-priv.tags["Name"]
}
output "KKE_subnet_name" {
    value = aws_subnet.datacenter-priv.tags["Name"]
}
output "KKE_ec2_private" {
    value = aws_instance.datacenter-priv.tags["Name"]
}

```
