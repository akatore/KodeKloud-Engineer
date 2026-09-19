
The Nautilus DevOps team has some volumes in different regions in their AWS account. They are going to setup some automated backups so that all important data can be backed up on regular basis. For now they shared some requirements to take a snapshot of one of the volumes they have.

Create a snapshot of an existing volume named `devops-vol` in `us-east-1` region using `terraform`.

1) The name of the snapshot must be `devops-vol-ss`.

2) The description must be `Devops Snapshot`.

3) Make sure the snapshot status is `completed` before submitting the task.

The Terraform working directory is `/home/bob/terraform`. Update the `main.tf` file (do not create a separate `.tf` file) to accomplish this task.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.

```
resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"

  tags = {
    Name        = "devops-vol"
  }
}

resource "aws_ebs_snapshot" "devops-vol-ss" {
  volume_id = aws_ebs_volume.k8s_volume.id
  description = "Devops Snapshot"
  tags = {
    Name = "devops-vol-ss"
  }
}

output "aws_ebs_snapshot_volume_size" {
  value = aws_ebs_snapshot.devops-vol-ss.volume_size
}
output "aws_ebs_snapshot_id" {
  value = aws_ebs_snapshot.devops-vol-ss.id
}
output "aws_ebs_snapshot_arn" {
  value = aws_ebs_snapshot.devops-vol-ss.arn
}

```




-----

The Nautilus DevOps team has some volumes in different regions in their AWS account. They are going to setup some automated backups so that all important data can be backed up on regular basis. For now they shared some requirements to take a snapshot of one of the volumes they have.

Create a snapshot of an existing volume named `xfusion-vol` in `us-east-1` region using `terraform`.

1) The name of the snapshot must be `xfusion-vol-ss`.

2) The description must be `Xfusion Snapshot`.

3) Make sure the snapshot status is `completed` before submitting the task.

The Terraform working directory is `/home/bob/terraform`. Update the `main.tf` file (do not create a separate `.tf` file) to accomplish this task.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.


```
resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"

  tags = {
    Name        = "xfusion-vol"
  }
}

resource "aws_ebs_snapshot" "xfusion-vol-ss" {
  volume_id = aws_ebs_volume.k8s_volume.id
  description = "Xfusion Snapshot"
  tags = {
    Name = "xfusion-vol-ss"
  }
}


```
<img width="696" height="724" alt="image" src="https://github.com/user-attachments/assets/c860bdab-2a7f-47df-b60b-40ce7002cce2" />

### After adding output:
```resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"

  tags = {
    Name        = "xfusion-vol"
  }
}

resource "aws_ebs_snapshot" "xfusion-vol-ss" {
  volume_id = aws_ebs_volume.k8s_volume.id
  description = "Xfusion Snapshot"
  tags = {
    Name = "xfusion-vol-ss"
  }
}

output "aws_ebs_snapshot_id" {
  value = aws_ebs_snapshot.xfusion-vol-ss.id
}

output "aws_ebs_snapshot_arn" {
  value = aws_ebs_snapshot.xfusion-vol-ss.arn
}

output "aws_ebs_snapshot_volume_size" {
  value = aws_ebs_snapshot.xfusion-vol-ss.volume_size
}
```
<img width="775" height="515" alt="image" src="https://github.com/user-attachments/assets/053766e6-0e8f-46a5-addf-7bb6c79c3fd8" />

https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ebs_snapshot
