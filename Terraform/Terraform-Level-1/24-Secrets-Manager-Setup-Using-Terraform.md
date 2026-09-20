The Nautilus DevOps team needs to store sensitive data securely using AWS Secrets Manager. They need to create a secret with the following specifications:

1) The secret name should be `datacenter-secret`.

2) The secret value should contain a key-value pair with `username: admin` and `password: Namin123`.

3) Use `Terraform` to create the secret in AWS Secrets Manager.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

this code won't work as we have put it into the tags which are publicly avaialable,
```
resource "aws_secretsmanager_secret" "datacenter-secret" {
    name = "datacenter-secret"
    tags = {
        username = "admin"
        password = "Namin123"
    }
}
```

### This is what gonna work..
```
resource "aws_secretsmanager_secret" "datacenter-secret" {
    name = "datacenter-secret"
}

# The map here can come from other supported configurations
# like locals, resource attribute, map() built-in, etc.
variable "secret_content" {
    default = {
        username = "admin"
        password = "Namin123"
        }
        type = map(string)
}

# Inject the actual encrypted key-value pair value into the container
resource "aws_secretsmanager_secret_version" "datacenter-secret" {
    secret_id     = aws_secretsmanager_secret.datacenter-secret.id
    secret_string = jsonencode(var.secret_content)
}

output "secret_arn" {
    value = aws_secretsmanager_secret.datacenter-secret.arn
}
```
<img width="800" height="601" alt="image" src="https://github.com/user-attachments/assets/45b4936d-008a-4efc-be27-bf42dbf67e77" />

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret#tags-1)
