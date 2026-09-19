The mariyam DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.

Create an IAM group named `iamgroup_mariyam` using `terraform`.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

```
resource "aws_iam_group" "iamgroup_mariyam" {
  name = "iamgroup_mariyam"
}

```
<img width="530" height="256" alt="image" src="https://github.com/user-attachments/assets/28f4a15c-1d17-4c55-b4f6-69348df1c1b3" />
