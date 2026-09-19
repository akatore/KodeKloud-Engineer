As part of the data migration process, the Nautilus DevOps team is actively creating several S3 buckets on AWS. They plan to utilize both private and public S3 buckets to store the relevant data. Given the ongoing migration of other infrastructure to AWS, it is logical to consolidate data storage within the AWS environment as well.

1. **Create a `public` S3 bucket** named `datacenter-s3-669075427` using Terraform.
2. **Ensure the bucket is accessible publicly once created** by setting the proper ACL.
    
    The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.
    

**Notes:**

- Create the resources only in the `us-east-1` region.
- Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.
- The name of the S3 bucket should be based on `datacenter-s3-669075427`.
- You can use the `ACL` settings to ensure the bucket is publicly accessible.

```
resource "aws_s3_bucket" "datacenter-s3-669075427" {
  bucket = "datacenter-s3-669075427"
}

resource "aws_s3_bucket_ownership_controls" "example" {
  bucket = aws_s3_bucket.datacenter-s3-669075427.id
  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}

resource "aws_s3_bucket_public_access_block" "example" {
  bucket = aws_s3_bucket.datacenter-s3-669075427.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_acl" "example" {
  depends_on = [
    aws_s3_bucket_ownership_controls.example,
    aws_s3_bucket_public_access_block.example,
  ]

  bucket = aws_s3_bucket.datacenter-s3-669075427.id
  acl    = "public-read"
}

```

<img width="663" height="275" alt="image" src="https://github.com/user-attachments/assets/7add0831-edc5-4a5e-b0e0-370686f4ae01" />
