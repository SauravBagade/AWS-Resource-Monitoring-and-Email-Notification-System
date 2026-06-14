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
In this project, these AWS services are used:

| Service            | Free Tier / Cost                                                                           |
| ------------------ | ------------------------------------------------------------------------------------------ |
| Amazon SNS         | Very low cost. Email notifications are usually a few cents for many thousands of messages. |
| AWS Lambda         | 1 million free requests/month. Personal projects usually stay in free tier.                |
| Amazon EventBridge | Free tier available; low cost after that. Personal usage is usually negligible.            |
| AWS CloudTrail     | Event History is free. Additional trails and S3 log storage can incur charges.             |
| Amazon S3          | Charged for storage used by CloudTrail logs. Usually only a few MB for small projects.     |
| IAM                | No additional charge.                                                                      |

### Estimated Monthly Cost

For a learning project with:

* 10–50 AWS resource changes per day
* 10–50 emails per day
* Small CloudTrail log volume

Approximate cost:

```text
IAM            = $0
Lambda         = $0 (Free Tier)
EventBridge    = $0 (Free Tier)
SNS Email      = ~$0
CloudTrail     = ~$0
S3 Storage     = <$0.10
```

### Realistic Total

For a personal DevOps portfolio project:

```text
$0.00 - $1.00 per month
```
---
# Author

Saurav Bagade

DevOps Engineer
