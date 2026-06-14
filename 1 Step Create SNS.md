# Step 1: Create SNS Topic (Amazon Simple Notification Service)

## Objective

Create an SNS Topic that will be used by AWS Lambda to send email notifications whenever AWS resources are created, deleted, or modified.

---

# What is SNS?

Amazon SNS (Simple Notification Service) is a fully managed messaging service that allows applications and AWS services to send notifications.

In this project:

```text
Lambda → SNS → Email
```

Lambda sends a message to SNS, and SNS delivers that message to your email address.

---

# Prerequisites

Before creating the SNS topic:

* AWS Account
* AWS Console Access
* Email Address for receiving notifications
* Appropriate IAM permissions

---

# Login to AWS Console

1. Open your browser.
2. Go to AWS Console.
3. Sign in using your AWS account.

---

# Open SNS Service

### Method 1

1. In the AWS Console search bar, type:

```text
SNS
```

2. Click:

```text
Simple Notification Service
```

---

### Method 2

Navigate manually:

```text
AWS Console
→ Services
→ Application Integration
→ Simple Notification Service (SNS)
```

---

# Navigate to Topics

From the SNS dashboard:

1. Click:

```text
Topics
```

Located on the left navigation pane.

---

# Create Topic

Click:

```text
Create Topic
```

---

# Topic Details

You will see the Topic Creation page.

---

## Topic Type

Choose:

```text
Standard
```

### Why Standard?

Standard topics provide:

* High throughput
* Best effort ordering
* Low latency
* Supports email notifications

For this project:

```text
Standard = Recommended
```

---

## FIFO Topic

Do NOT select:

```text
FIFO
```

Reason:

FIFO topics are only required when:

* Message ordering is important
* Duplicate prevention is required

Not needed for alert notifications.

---

# Topic Name

Enter:

```text
aws-resource-alerts
```

### Naming Rules

* Letters allowed
* Numbers allowed
* Hyphens allowed
* No spaces

Examples:

```text
aws-resource-alerts
aws-monitoring-alerts
resource-notification-topic
```

---

# Display Name (Optional)

Enter:

```text
AWS Alerts
```

Purpose:

The display name may appear in emails received from SNS.

Example:

```text
From: AWS Alerts
```

---

# Encryption

Locate:

```text
Encryption
```

Choose:

```text
Disable Encryption
```

or leave default.

### Why?

This project does not require encryption.

Encryption can be added later using:

```text
AWS KMS
```

---

# Access Policy

Locate:

```text
Access Policy
```

Keep:

```text
Basic
```

Default settings are sufficient.

---

# Data Protection

Leave:

```text
Disabled
```

Default configuration is fine.

---

# Delivery Policy

Leave:

```text
Default
```

No customization required.

---

# Message Archiving

Leave:

```text
Disabled
```

Not required for this project.

---

# Tags (Optional)

You may leave blank.

Or add:

| Key         | Value                 |
| ----------- | --------------------- |
| Project     | AWSResourceMonitoring |
| Environment | Demo                  |
| Owner       | Saurav                |

Tags help identify resources.

---

# Active Tracing

Locate:

```text
Active Tracing
```

Choose:

```text
Disabled
```

Not required.

---

# Delivery Status Logging

Locate:

```text
Delivery Status Logging
```

Keep:

```text
Disabled
```

No need for additional logging.

---

# Review Configuration

Verify:

| Setting                 | Value               |
| ----------------------- | ------------------- |
| Topic Type              | Standard            |
| Topic Name              | aws-resource-alerts |
| Display Name            | AWS Alerts          |
| Encryption              | Disabled            |
| Access Policy           | Default             |
| Delivery Policy         | Default             |
| Active Tracing          | Disabled            |
| Delivery Status Logging | Disabled            |

---

# Create Topic

Click:

```text
Create Topic
```

AWS will create the SNS topic.

---

# Verify Topic Creation

After creation:

Navigate to:

```text
SNS
→ Topics
→ aws-resource-alerts
```

Verify:

```text
Status = Active
```

---

# Copy Topic ARN

Locate:

```text
Topic ARN
```

Example:

```text
arn:aws:sns:us-east-1:123456789012:aws-resource-alerts
```

Copy this ARN.

You will use it later in:

```text
Lambda Environment Variables
```

Store it safely.

---

# Expected Output

After successful creation:

You should have:

```text
Topic Name:
aws-resource-alerts

Topic Type:
Standard

Status:
Active

Topic ARN:
arn:aws:sns:region:account-id:aws-resource-alerts
```

---
