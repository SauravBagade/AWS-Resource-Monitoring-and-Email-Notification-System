# AWS Resource Monitoring and Email Notification System

## Project Overview

This project monitors AWS resource activity and sends email notifications whenever AWS resources are created, deleted, or modified.

The solution uses a fully serverless architecture:

```text
CloudTrail
    ↓
EventBridge
    ↓
Lambda
    ↓
SNS
    ↓
Email Notification
```

---

# Architecture

```text
+------------+
| CloudTrail |
+------------+
       |
       v
+-------------+
| EventBridge |
+-------------+
       |
       v
+---------+
| Lambda  |
+---------+
       |
       v
+------+
| SNS  |
+------+
       |
       v
+-------------+
| Email Alert |
+-------------+
```

---

# AWS Services Used

| Service     | Purpose                  |
| ----------- | ------------------------ |
| CloudTrail  | Capture AWS API activity |
| EventBridge | Detect CloudTrail events |
| Lambda      | Process event data       |
| SNS         | Send email notifications |
| IAM         | Manage permissions       |
| S3          | Store CloudTrail logs    |

---

# Prerequisites

* AWS Account
* IAM User with Administrator Access
* Verified Email Address
* Basic AWS Knowledge

---

# Expected Output

Whenever a monitored AWS resource is created or deleted:

* CloudTrail captures event
* EventBridge matches event
* Lambda executes
* SNS sends email
* User receives notification

---

# Future Enhancements

* Slack Notifications
* Microsoft Teams Alerts
* HTML Email Templates
* Terraform Deployment
* CloudWatch Dashboard
* Multi-Account Monitoring
* Cost Monitoring
* Resource Tag Tracking

---

# Author

Saurav Bagade

DevOps Engineer
