# Step 7: Create EventBridge Rule

## Objective

Create an Amazon EventBridge Rule that listens for AWS CloudTrail events and automatically triggers the Lambda function whenever a resource is created or deleted.

After completing this step:

```text
CloudTrail
    ↓
EventBridge
    ↓
Lambda
```

will be working.

---

# What is EventBridge?

Amazon EventBridge is an event bus service.

It receives events from AWS services and routes them to targets.

Example:

```text
CloudTrail Event
        ↓
EventBridge Rule
        ↓
Lambda Function
```

EventBridge acts as the bridge between CloudTrail and Lambda.

---

# Architecture

Before Step 7:

```text
CloudTrail
```

After Step 7:

```text
CloudTrail
    ↓
EventBridge
    ↓
Lambda
```

Final Architecture:

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
✓ Step 6: Create CloudTrail Trail
```

---

# Open EventBridge

Login to AWS Console.

Search:

```text
EventBridge
```

Open:

```text
Amazon EventBridge
```

---

# Open Rules

Navigate:

```text
EventBridge
→ Rules
```

---

# Create Rule

Click:

```text
Create Rule
```

---

# Define Rule Detail

## Name

Enter:

```text
aws-resource-alert-rule
```

---

## Description (Optional)

Example:

```text
Triggers Lambda whenever CloudTrail detects AWS resource creation or deletion events.
```

---

## Event Bus

Select:

```text
default
```

Why?

CloudTrail automatically sends events to the default EventBridge bus.

---

## Rule Type

Choose:

```text
Rule with an Event Pattern
```

Do NOT choose:

```text
Schedule
```

because we need event-driven execution.

---

# Build Event Pattern

Under:

```text
Event Pattern
```

Select:

```text
Custom Pattern (JSON Editor)
```

---

# Paste Event Pattern

Paste:

```json
{
  "detail-type": ["AWS API Call via CloudTrail"],
  "detail": {
    "eventName": [
      "RunInstances",
      "TerminateInstances",
      "CreateBucket",
      "DeleteBucket",
      "CreateUser",
      "DeleteUser",
      "CreateRole",
      "DeleteRole",
      "CreateDBInstance",
      "DeleteDBInstance"
    ]
  }
}
```

---

# Event Pattern Explanation

## RunInstances

Detect:

```text
EC2 Instance Creation
```

---

## TerminateInstances

Detect:

```text
EC2 Instance Deletion
```

---

## CreateBucket

Detect:

```text
S3 Bucket Creation
```

---

## DeleteBucket

Detect:

```text
S3 Bucket Deletion
```

---

## CreateUser

Detect:

```text
IAM User Creation
```

---

## DeleteUser

Detect:

```text
IAM User Deletion
```

---

## CreateRole

Detect:

```text
IAM Role Creation
```

---

## DeleteRole

Detect:

```text
IAM Role Deletion
```

---

## CreateDBInstance

Detect:

```text
RDS Instance Creation
```

---

## DeleteDBInstance

Detect:

```text
RDS Instance Deletion
```

---

# Click Next

Click:

```text
Next
```

---

# Select Target

EventBridge now asks:

```text
Select Target
```

---

# Target Type

Choose:

```text
AWS Service
```

---

# Service

Choose:

```text
Lambda Function
```

---

# Function

Select:

```text
aws-resource-notifier
```

Verify:

```text
Lambda Function Name:
aws-resource-notifier
```

---

# Additional Settings

Leave defaults.

No transformation required.

No dead-letter queue required.

No retry policy changes required.

---

# Click Next

Click:

```text
Next
```

---

# Configure Tags

Optional.

Example:

| Key         | Value                 |
| ----------- | --------------------- |
| Project     | AWSResourceMonitoring |
| Environment | Demo                  |

You may skip this section.

---

# Review Rule

Verify:

| Setting         | Value                   |
| --------------- | ----------------------- |
| Rule Name       | aws-resource-alert-rule |
| Event Bus       | default                 |
| Rule Type       | Event Pattern           |
| Target          | Lambda                  |
| Lambda Function | aws-resource-notifier   |

---

# Create Rule

Click:

```text
Create Rule
```

AWS creates the EventBridge rule.

---

# Verify Rule Creation

Navigate:

```text
EventBridge
→ Rules
```

Open:

```text
aws-resource-alert-rule
```

---

# Verify Status

Status should be:

```text
Enabled
```

NOT:

```text
Disabled
```

---

# Verify Target

Under:

```text
Targets
```

Verify:

```text
aws-resource-notifier
```

appears.

---

# Verify Lambda Trigger

Navigate:

```text
Lambda
→ aws-resource-notifier
```

Open:

```text
Configuration
→ Triggers
```

Verify:

```text
EventBridge
```

appears as a trigger.

Example:

```text
EventBridge
aws-resource-alert-rule
```

---

# Test EventBridge

Create:

```text
S3 Bucket
```

Example:

```text
test-alert-bucket-12345
```

Wait:

```text
1-2 Minutes
```

---

# Verify Lambda Invocation

Navigate:

```text
Lambda
→ aws-resource-notifier
→ Monitor
```

Check:

```text
Invocations
```

Value should increase.

---

# Verify Email Notification

Check:

```text
Inbox
Spam
Promotions
```

Expected Email:

```text
AWS Resource Notification

Action  : Created
Service : s3.amazonaws.com
Event   : CreateBucket
Region  : us-east-1
Time    : 2026-06-14T10:00:00Z
```

---

# Validation Checklist

Verify:

```text
✓ EventBridge Opened
✓ Rules Opened
✓ Create Rule Clicked
✓ Rule Name Added
✓ Event Bus Selected
✓ Event Pattern Added
✓ Lambda Selected
✓ Rule Created
✓ Rule Status Enabled
✓ Lambda Trigger Visible
✓ Event Triggered
✓ Email Received
```

---

# Common Errors

## Rule Disabled

Solution:

```text
EventBridge
→ Rules
→ Enable Rule
```

---

## Lambda Not Triggered

Verify:

```text
Target = aws-resource-notifier
```

---

## Wrong Event Pattern

Verify JSON exactly matches:

```json
{
  "detail-type": ["AWS API Call via CloudTrail"]
}
```

or the filtered version above.

---

## No Email Received

Check:

```text
Lambda Logs
SNS Subscription Status
Environment Variable
```

---

# Expected Output

You should have:

```text
Rule Name:
aws-resource-alert-rule

Status:
Enabled

Target:
aws-resource-notifier

Event Source:
CloudTrail
```

---

# Architecture After Step 7

```text
CloudTrail
    ↓
EventBridge
    ↓
Lambda
```

EventBridge is now automatically triggering Lambda whenever selected AWS resource events occur.

---

# Next Step

Proceed to:

```text
Step 8: End-to-End Testing and Validation
```

This step verifies the complete flow:

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
