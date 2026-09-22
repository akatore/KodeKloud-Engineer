The Nautilus DevOps team is automating IAM policy creation using Terraform to enhance security and access management. As part of this task, they need to create an IAM policy with specific requirements.

For this task, create an AWS IAM policy using `Terraform` with the following requirements:

1. The IAM policy name `iampolicy_anita` should be stored in a variable named `KKE_iampolicy`.

**Note:**

1. The configuration values should be stored in a `variables.tf` file.
2. The Terraform script should be structured with a `main.tf` file referencing `variables.tf`.
3. The Terraform working directory is `/home/bob/terraform`.
4. Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

```
resource "aws_iam_policy" "iampolicy_anita" {
  name        = var.KKE_iampolicy
  path        = "/"
  description = "My test policy"

  # Terraform's "jsonencode" function converts a
  # Terraform expression result to valid JSON syntax.
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "ec2:Describe*",
        ]
        Effect   = "Allow"
        Resource = "*"
      },
    ]
  })
}
```

```
variable "KKE_iampolicy" {
    default = "iampolicy_anita"
    type = string
}
```
