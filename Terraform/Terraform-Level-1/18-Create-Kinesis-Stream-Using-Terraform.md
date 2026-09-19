The Nautilus DevOps team needs to create an AWS Kinesis data stream for real-time data processing. This stream will be used to ingest and process large volumes of streaming data, which will then be consumed by various applications for analytics and real-time decision-making.

1. The stream should be named `datacenter-stream`.
2. Use `Terraform` to create this Kinesis stream.
    
    The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.
    
    `Note:`
    
    1. Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.
    2. Before submitting the task, ensure that `terraform plan` returns `No changes. Your infrastructure matches the configuration.`
  
```
resource "aws_kinesis_stream" "datacenter-stream" {
  name             = "datacenter-stream"
  shard_count      = 1
}
```

Others field are optional, above is the bare minimum.

<img width="819" height="593" alt="image" src="https://github.com/user-attachments/assets/61a34ec9-5501-4fc7-ba93-971999bcc513" />



[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kinesis_stream)
