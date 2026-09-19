As part of the data migration process, the Nautilus DevOps team is actively creating several S3 buckets on AWS using Terraform. They plan to utilize both private and public S3 buckets to store the relevant data. Given the ongoing migration of other infrastructure to AWS, it is logical to consolidate data storage within the AWS environment as well.

Create an S3 bucket using Terraform with the following details:

1) The name of the S3 bucket must be `xfusion-s3-276598094`.

2) The S3 bucket must block all `public` access, making it a private bucket.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

**Notes:**

- Use Terraform to provision the S3 bucket.
- Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.
- Ensure the resources are created in the `us-east-1` region.
- The bucket must have block public access enabled to restrict any public access.
```
resource "aws_s3_bucket" "xfusion-s3-276598094" {
  bucket = "xfusion-s3-276598094"
}

resource "aws_s3_bucket_ownership_controls" "xfusion-s3-276598094" {
  bucket = aws_s3_bucket.xfusion-s3-276598094.id
  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}

resource "aws_s3_bucket_acl" "xfusion-s3-276598094" {
  depends_on = [aws_s3_bucket_ownership_controls.xfusion-s3-276598094]

  bucket = aws_s3_bucket.xfusion-s3-276598094.id
  acl    = "private"
}

```
<img width="673" height="143" alt="image" src="https://github.com/user-attachments/assets/5a506c08-0ef6-45a7-9eac-22f01fa991cd" />
