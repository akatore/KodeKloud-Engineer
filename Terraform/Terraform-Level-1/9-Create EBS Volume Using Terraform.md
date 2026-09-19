The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create an AWS EBS volume using Terraform with the following requirements:

- Name of the volume should be **`datacenter-volume`**.
- Volume **type** must be **`gp3`**.
- Volume **size** must be **`2 GiB`**.
- Ensure the volume is created in **`us-east-1`**.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

As per question this, but AZ is us-east-1a, us-east-1b, us-east-1c...
```
resource "aws_ebs_volume" "datacenter-volume" {
    size = 2
    availability_zone = "us-east-1"
    type = "gp3"
    tags = {
        Name = "datacenter-volume"
    }
}

output "ebs_id" {
    value = aws_ebs_volume.datacenter-volume.id
}
```
Doing `terraform apply` will keep creating the new ebs.

<img width="787" height="673" alt="image" src="https://github.com/user-attachments/assets/3eb3af5b-5b0c-4a00-b819-cf0b21327a82" />


<img width="791" height="705" alt="image" src="https://github.com/user-attachments/assets/3bc0096d-299f-448a-8bd9-9adcbabaf1c8" />



Below This is correct actually, and it wont keep creating new EBS every time

```
resource "aws_ebs_volume" "datacenter-volume" {
    size = 2
    availability_zone = "us-east-1a"
    type = "gp3"
    tags = {
        Name = "datacenter-volume"
    }
}

output "ebs_id" {
    value = aws_ebs_volume.datacenter-volume.id
}
```
<img width="728" height="275" alt="image" src="https://github.com/user-attachments/assets/44226d2a-1870-4aca-9afb-53da2bca642e" />
