The Nautilus DevOps team is setting up monitoring in their AWS account. As part of this, they need to create a CloudWatch alarm.

Using `Terraform`, perform the following:

### **Task Details:**

1. Create a **CloudWatch alarm** named `datacenter-alarm`.
2. The alarm should monitor **CPU utilization** of an EC2 instance.
3. Trigger the alarm when **CPU utilization exceeds 80%**.
4. Set the **evaluation period** to **5 minutes**.
5. Use a **single evaluation period**.

Ensure that the entire configuration is implemented using `Terraform`. The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

`Note:` Right-click under the `EXPLORER` section in `VS Code` and select `Open in Integrated Terminal` to launch the terminal.


```
resource "aws_cloudwatch_metric_alarm" "datacenter-alarm" {
  alarm_name                = "datacenter-alarm"
  statistic                 = "Average" 
  comparison_operator       = "GreaterThanOrEqualToThreshold"
  evaluation_periods        = 1
  metric_name               = "CPUUtilization"
  namespace                 = "AWS/EC2"
  period                    = 300
  threshold                 = 80
  alarm_description         = "This metric monitors ec2 cpu utilization"
}

```

<img width="695" height="507" alt="image" src="https://github.com/user-attachments/assets/d7cedce6-97eb-4bb1-840d-acae01ba2e6f" />
