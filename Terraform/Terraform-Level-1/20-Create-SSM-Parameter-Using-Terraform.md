The Nautilus DevOps team needs to create an SSM parameter in AWS with the following requirements:

1) The name of the parameter should be `devops-ssm-parameter`.

2) Set the parameter type to `String`.

3) Set the parameter value to `devops-value`.

4) The parameter should be created in the `us-east-1` region.

5) Ensure the parameter is successfully created using `terraform` and can be retrieved when the task is completed.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

```
terraform init; terraform plan; terraform apply --auto-approve
```

```
resource "aws_ssm_parameter" "devops-ssm-parameter" {
  name  = "devops-ssm-parameter"
  type  = "String"
  value = "devops-value"
}

```
<img width="812" height="585" alt="image" src="https://github.com/user-attachments/assets/b3b6a752-70b8-437d-afb2-ddbd4da91c4d" />

<img width="625" height="503" alt="image" src="https://github.com/user-attachments/assets/1e1457b4-7b44-4e64-9171-64db4a862f61" />

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ssm_parameter)
