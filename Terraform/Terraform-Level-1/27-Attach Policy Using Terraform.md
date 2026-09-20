The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.

An IAM user named `iamuser_jim` and a policy named `iampolicy_jim` already exists. Use `Terraform` to attach the IAM policy `iampolicy_jim` to the IAM user `iamuser_jim`. The Terraform working directory is `/home/bob/terraform`. Update the `main.tf` file (do not create a separate `.tf` file) to attach the specified IAM policy to the IAM user.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.


```
# Create IAM user
resource "aws_iam_user" "user" {
  name = "iamuser_jim"

  tags = {
    Name = "iamuser_jim"
  }
}

# Create IAM Policy
resource "aws_iam_policy" "policy" {
  name        = "iampolicy_jim"
  description = "IAM policy allowing EC2 read actions for jim"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect   = "Allow"
        Action   = ["ec2:Read*"]
        Resource = "*"
      }
    ]
  })
}

resource "aws_iam_user_policy_attachment" "user_attach" {
  user       = aws_iam_user.user.name
  policy_arn = aws_iam_policy.policy.arn
}

```

<img width="787" height="480" alt="image" src="https://github.com/user-attachments/assets/2218ccfb-77e2-4805-8d02-6a1e0dfd08cf" />

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_user_policy_attachment)
