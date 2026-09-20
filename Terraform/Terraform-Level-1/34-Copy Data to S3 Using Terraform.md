The Nautilus DevOps team is presently immersed in data migrations, transferring data from on-premise storage systems to AWS S3 buckets. They have recently received some data that they intend to copy to one of the S3 buckets.

S3 bucket named nautilus-cp-293103898 already exists. Copy the file /tmp/nautilus.txt to s3 bucket nautilus-cp-293103898 using Terraform. The Terraform working directory is /home/bob/terraform. Update the main.tf file (do not create a separate .tf file) to accomplish this task.

Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

```
resource "aws_s3_bucket" "my_bucket" {
  bucket = "nautilus-cp-293103898"
  acl    = "private"

  tags = {
    Name        = "nautilus-cp-293103898"
  }
}

resource "aws_s3_object" "nautilus_file" {
  bucket = aws_s3_bucket.my_bucket.id
  key    = "nautilus.txt"        # The name the file will have inside the S3 bucket
  source = "/tmp/nautilus.txt"   # The path to the local file on your machine
}
```

<img width="746" height="474" alt="image" src="https://github.com/user-attachments/assets/3f0d39ad-7c85-494e-9522-a0fd2cf84348" />
