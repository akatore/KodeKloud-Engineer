Data protection and recovery are fundamental aspects of data management. It's essential to have systems in place to ensure that data can be recovered in case of accidental deletion or corruption. The DevOps team has received a requirement for implementing such measures for one of the S3 buckets they are managing.

The S3 bucket name is `nautilus-s3-822520266`, enable `versioning` for this bucket using `Terraform`.

The Terraform working directory is `/home/bob/terraform`. Update the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

```
resource "aws_s3_bucket" "s3_ran_bucket" {
  bucket = "nautilus-s3-822520266"
  acl    = "private"

  tags = {
    Name        = "nautilus-s3-822520266"
  }
}


resource "aws_s3_bucket_acl" "example" {
  bucket = aws_s3_bucket.s3_ran_bucket.id
  acl    = "private"
}

resource "aws_s3_bucket_versioning" "versioning_example" {
  bucket = aws_s3_bucket.s3_ran_bucket.id
  versioning_configuration {
    status = "Enabled"
  }
}

```
<img width="789" height="392" alt="image" src="https://github.com/user-attachments/assets/d6374670-f4f6-4ef0-8afb-3d73e380781a" />

[reference](https://registry.terraform.io/providers/-/aws/latest/docs/resources/s3_bucket_versioning)
