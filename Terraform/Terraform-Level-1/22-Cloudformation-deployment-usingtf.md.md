The Nautilus DevOps team is working on automating infrastructure deployment using AWS CloudFormation. As part of this effort, they need to create a CloudFormation stack that provisions an S3 bucket with versioning enabled.

Create a CloudFormation stack named devops-stack using Terraform. This stack should contain an S3 bucket named devops-bucket-645401085 as a resource, and the bucket must have versioning enabled. The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.

Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

```
resource "aws_cloudformation_stack" "devops-stack" {
  name = "devops-stack"

  template_body = jsonencode({
    Resources = {
      S3Bucket = {
        Type = "AWS::S3::Bucket"
        Properties = {
          BucketName = "devops-bucket-645401085"
          VersioningConfiguration = {
            Status = "Enabled"
          }
        }
      }
    }
  })
}
```
<img width="1050" height="797" alt="image" src="https://github.com/user-attachments/assets/56a65321-73a0-4569-94ff-bfb3d8683fe0" />


https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudformation_stack
