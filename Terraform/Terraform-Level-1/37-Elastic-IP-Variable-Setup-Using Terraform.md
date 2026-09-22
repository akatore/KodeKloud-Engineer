The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. As part of this phased migration approach, they need to allocate an Elastic IP address to support external access for specific workloads.

For this task, create an AWS Elastic IP using `Terraform` with the following requirement:

1. The Elastic IP name `nautilus-eip` should be stored in a variable named `KKE_eip`. The Terraform working directory is `/home/bob/terraform`.

**Note:**

1. The configuration values should be stored in a `variables.tf` file.
2. The Terraform script should be structured with a `main.tf` file referencing `variables.tf`.
3. Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.


```
resource "aws_eip" "nautilus" {
    domain = "vpc"
    tags = {
        Name = var.KKE_eip
    }
}
```

```
variable "KKE_eip" {
    default = "nautilus-eip"
    type = string
}
```
