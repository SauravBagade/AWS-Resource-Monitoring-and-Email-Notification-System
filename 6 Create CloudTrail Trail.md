# Step 6: Create CloudTrail Trail

## Objective

Create an AWS CloudTrail Trail to capture AWS API activity across your AWS account.

CloudTrail records actions such as:

* EC2 Instance Creation
* EC2 Instance Termination
* S3 Bucket Creation
* S3 Bucket Deletion
* IAM User Creation
* IAM User Deletion
* RDS Instance Creation
* RDS Instance Deletion

These events will later be sent to EventBridge and processed by Lambda.

---

# What is CloudTrail?

AWS CloudTrail is an auditing and monitoring service.

It records every API call made within your AWS account.

Example:

```text
User Creates EC2
        ↓
AWS API Call
        ↓
CloudTrail Records Event
```

Example Event:

```text
RunInstances
TerminateInstances
CreateBucket
DeleteBucket
CreateUser
DeleteUser
```

---

# Architecture

After completing this step:

```text
CloudTrail
    ↓
Stores AWS API Events
```

Later:

```text
CloudTrail
    ↓
EventBridge
    ↓
Lambda
    ↓
SNS
    ↓
Email
```

---

# Prerequisites

Complete:

```text
✓ Step 1: Create SNS Topic
✓ Step 2: Create SNS Subscription
✓ Step 3: Create IAM Role
✓ Step 4: Add SNS Publish Permission
✓ Step 5: Create Lambda Function
```

---

# Open CloudTrail

Login to AWS Console.

Search:

```text
CloudTrail
```

Open:

```text
AWS CloudTrail
```

---

# Create Trail

Click:

```text
Create Trail
```

---

# General Details

## Trail Name

Enter:

```text
aws-resource-trail
```

Naming Rules:

* Letters allowed
* Numbers allowed
* Hyphen allowed

Example:

```text
aws-resource-trail
```

---

# Storage Location

CloudTrail stores logs in Amazon S3.

Choose:

```text
Create New S3 Bucket
```

AWS automatically creates the bucket.

Example:

```text
aws-cloudtrail-logs-123456789012-abcdef
```

No manual bucket creation required.

---

# S3 Bucket Name

Leave:

```text
Generate New Bucket
```

AWS creates the bucket automatically.

---

# Log File SSE Encryption

Keep:

```text
Enabled
```

Default setting.

This encrypts CloudTrail log files.

---

# Log File Validation

Keep:

```text
Enabled
```

Purpose:

Detect log tampering.

Recommended by AWS.

---

# SNS Notification Delivery

Leave:

```text
Disabled
```

Not required.

Reason:

Our project already uses:

```text
CloudTrail
    ↓
EventBridge
    ↓
Lambda
    ↓
SNS
```

---

# CloudWatch Logs

Leave:

```text
Disabled
```

Not required for this project.

---

# Trail Scope

Choose:

```text
Apply Trail to All Regions
```

Also known as:

```text
Multi-region Trail
```

---

# Why Multi-Region?

Without Multi-Region:

```text
Only One Region Monitored
```

With Multi-Region:

```text
All AWS Regions Monitored
```

Recommended for production.

---

# Management Events

Locate:

```text
Management Events
```

Select:

```text
Read
Write
```

Enable both.

---

# What Are Read Events?

Examples:

```text
DescribeInstances
GetBucketPolicy
ListUsers
```

---

# What Are Write Events?

Examples:

```text
RunInstances
TerminateInstances
CreateBucket
DeleteBucket
CreateUser
DeleteUser
```

Write events are the most important for this project.

---

# Data Events

Leave:

```text
Disabled
```

Reason:

Data events generate large numbers of logs and additional costs.

Examples:

```text
S3 Object Upload
S3 Object Download
Lambda Invocation
```

Not required.

---

# Insights Events

Leave:

```text
Disabled
```

Optional feature.

Not required for resource monitoring.

---

# Review Configuration

Verify:

| Setting           | Value              |
| ----------------- | ------------------ |
| Trail Name        | aws-resource-trail |
| Multi Region      | Enabled            |
| New S3 Bucket     | Enabled            |
| Log Validation    | Enabled            |
| Encryption        | Enabled            |
| Management Events | Read + Write       |
| Data Events       | Disabled           |
| Insights Events   | Disabled           |

---

# Create Trail

Click:

```text
Create Trail
```

AWS creates:

```text
CloudTrail Trail
S3 Bucket
Logging Configuration
```

Automatically.

---

# Verify Trail Creation

After creation:

Navigate:

```text
CloudTrail
→ Trails
```

Open:

```text
aws-resource-trail
```

---

# Verify Status

Check:

```text
Logging
```

Status should be:

```text
Logging
```

NOT:

```text
Stopped
```

---

# Verify S3 Bucket

Under Storage Location verify:

```text
S3 Bucket
```

exists.

Example:

```text
aws-cloudtrail-logs-123456789012-abcdef
```

---

# Verify Event History

Open:

```text
CloudTrail
→ Event History
```

You should see recent AWS API calls.

Examples:

```text
CreateRole
CreateFunction
CreateTrail
RunInstances
```

This confirms CloudTrail is recording activity.

---

# Test CloudTrail

Create a simple resource.

Example:

```text
Create S3 Bucket
```

Wait:

```text
1-2 Minutes
```

Go to:

```text
CloudTrail
→ Event History
```

Search:

```text
CreateBucket
```

You should see the event.

---

# Validation Checklist

Verify:

```text
✓ CloudTrail Opened
✓ Create Trail Clicked
✓ Trail Name Added
✓ Multi-Region Enabled
✓ New S3 Bucket Created
✓ Encryption Enabled
✓ Log Validation Enabled
✓ Management Events Enabled
✓ Data Events Disabled
✓ Insights Disabled
✓ Trail Created
✓ Status = Logging
✓ Events Visible in Event History
```

---

# Common Errors

## Trail Status Not Logging

Solution:

```text
CloudTrail
→ Trails
→ Start Logging
```

---

## No Events Visible

Wait:

```text
1-5 Minutes
```

CloudTrail processing may take time.

---

## Data Events Enabled Accidentally

Problem:

```text
Higher Costs
```

Solution:

Disable Data Events.

---

# Expected Output

You should have:

```text
Trail Name:
aws-resource-trail

Status:
Logging

Scope:
Multi-Region

Storage:
S3 Bucket

Events:
Being Recorded Successfully
```

---

# Architecture After Step 6

```text
AWS Resource Activity
         ↓
CloudTrail
         ↓
Stores API Events
```

CloudTrail is now capturing AWS activity across your account.

---
