The Nautilus DevOps team needs to set up an Amazon OpenSearch Service domain to store and search their application logs. The domain should have the following specification:

1) The domain name should be `xfusion-es`.

2) Use `Terraform` to create the `OpenSearch` domain. The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

**Notes:**

1. The Terraform working directory is `/home/bob/terraform`.
2. Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.
3. Before submitting the task, ensure that `terraform plan` returns `No changes. Your infrastructure matches the configuration.`
4. The OpenSearch domain creation process may take several minutes. Please wait until the domain is fully created before submitting.

```
resource "aws_opensearch_domain" "xfusion-es" {
  domain_name    = "xfusion-es"
}
```
<img width="1054" height="824" alt="image" src="https://github.com/user-attachments/assets/0191a842-3a3c-4e5f-8e89-e2cec6d07f12" />

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/opensearch_domain)
