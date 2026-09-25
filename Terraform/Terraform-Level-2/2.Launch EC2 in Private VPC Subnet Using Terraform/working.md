### `main.tf`
```
resource "aws_vpc" "xfusion-priv" {
    cidr_block = var.KKE_VPC_CIDR
    tags = {
        Name = "xfusion-priv-vpc"
    }
}

resource "aws_subnet" "xfusion-priv" {
    vpc_id = aws_vpc.xfusion-priv.id
    cidr_block = var.KKE_SUBNET_CIDR
    tags = {
        Name = "xfusion-priv-subnet"
    }

}

resource "aws_security_group" "allow_tls" {
  name        = "allow_tls"
  description = "Allow TLS inbound traffic and all outbound traffic"
  vpc_id      = aws_vpc.xfusion-priv.id

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

resource "aws_instance" "xfusion-priv" {
    subnet_id = aws_subnet.xfusion-priv.id
    ami           = data.aws_ami.amazon_linux.id
    instance_type = "t2.micro"
    vpc_security_group_ids = [aws_security_group.allow_tls.id]
    tags = {
        Name = "xfusion-priv-ec2"
    }
}

```

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
```
### `outputs.tf`
```

output "KKE_vpc_name" {
  value = aws_vpc.xfusion-priv.tags["Name"]
}

output "KKE_subnet_name" {
  value = aws_subnet.xfusion-priv.tags["Name"]
}

output "KKE_ec2_private" {
  value = aws_instance.xfusion-priv.tags["Name"]
}

```


After apply always run plan to avoid if it says any things still pending creation...
<img width="1061" height="846" alt="image" src="https://github.com/user-attachments/assets/087b9667-661d-4c02-b5f7-69d5d8305ea4" />
