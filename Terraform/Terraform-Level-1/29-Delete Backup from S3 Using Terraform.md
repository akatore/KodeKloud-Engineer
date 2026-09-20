The Nautilus DevOps team is currently engaged in a cleanup process, focusing on removing unnecessary data and services from their AWS account. As part of the migration process, several resources were created for one-time use only, necessitating a cleanup effort to optimize their AWS environment.

A S3 bucket named `xfusion-bck-814910785` already exists.

1) Copy the contents of `xfusion-bck-814910785` S3 bucket to `/opt/s3-backup/` directory on `terraform-client` host (the landing host once you load this lab).

2) Delete the S3 bucket `xfusion-bck-814910785`.

3) Use the AWS CLI through Terraform to accomplish this task—for example, by running AWS CLI commands within Terraform. The Terraform working directory is `/home/bob/terraform`. Update the `main.tf` file (do not create a separate `.tf` file) to accomplish this task.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

```

```



----
To fulfill these requirements, you can use Terraform's `null_resource` combined with the `local-exec` provisioner. This allows you to execute local shell commands (like the AWS CLI) directly from within your Terraform apply step.

Here is the code you need to append to your `/home/bob/terraform/main.tf` file:

```hcl
resource "null_resource" "s3_cleanup" {
  provisioner "local-exec" {
    command = <<EOF
      # Copy all contents from the S3 bucket to the local directory
      aws s3 cp s3://xfusion-bck-814910785 /opt/s3-backup/ --recursive
      
      # Force delete the S3 bucket (which also empties any remaining objects)
      aws s3 rb s3://xfusion-bck-814910785 --force
    EOF
  }
}

```

<img width="789" height="690" alt="image" src="https://github.com/user-attachments/assets/56fe196a-f772-4e94-9e7e-1be51214b43c" />


### How it works:

1. **`null_resource`**: This is a special Terraform resource that doesn't create anything in AWS directly, but acts as a trigger to run provisioners.
2. **`local-exec`**: This tells Terraform to run the specified script on the local machine where Terraform is being executed (the `terraform-client` host).
3. **`aws s3 cp ... --recursive`**: This AWS CLI command recursively copies all objects from the bucket to your local `/opt/s3-backup/` folder.
4. **`aws s3 rb ... --force`**: This AWS CLI command deletes the bucket. The `--force` flag is critical here, as AWS requires a bucket to be completely empty before it can be deleted; `--force` automatically deletes all objects inside before removing the bucket itself.

### Steps to Apply:

Once you have saved the `main.tf` file, open your Integrated Terminal in VS Code, and run the following:

```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve

```

Terraform will initialize the `null` provider, run the AWS CLI commands, back up your files, and destroy the bucket.
