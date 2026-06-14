# Step 5: Create Lambda Function

## Objective

Create an AWS Lambda function that receives CloudTrail events from EventBridge, processes the event details, and sends email notifications through SNS.

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

# What is AWS Lambda?

AWS Lambda is a serverless compute service that runs code without managing servers.

For this project:

```text
EventBridge Event
        ↓
Lambda
        ↓
SNS
        ↓
Email
```

Lambda acts as the processing engine.

---

# Prerequisites

Before starting:

Complete:

```text
✓ Step 1: Create SNS Topic
✓ Step 2: Create SNS Subscription
✓ Step 3: Create IAM Role
✓ Step 4: Add SNS Publish Permission
```

---

# Open Lambda Service

Login to AWS Console.

Search:

```text
Lambda
```

Open:

```text
AWS Lambda
```

---

# Create Function

Click:

```text
Create Function
```

---

# Select Function Type

Choose:

```text
Author from Scratch
```

Do NOT select:

```text
Container Image
Blueprint
```

---

# Function Configuration

## Function Name

Enter:

```text
aws-resource-notifier
```

Naming Rules:

* Letters allowed
* Numbers allowed
* Hyphens allowed

---

## Runtime

Select:

```text
Python 3.12
```

Recommended runtime for this project.

---

## Architecture

Keep:

```text
x86_64
```

Default setting.

---

# Permissions

Expand:

```text
Change default execution role
```

---

## Execution Role

Select:

```text
Use an existing role
```

---

## Existing Role

Choose:

```text
aws-resource-notifier-role
```

or

```text
aws-resource-notifier-role-xxxx
```

Important:

Select the role configured in Step 4.

---

# Review Configuration

Verify:

| Setting        | Value                      |
| -------------- | -------------------------- |
| Function Name  | aws-resource-notifier      |
| Runtime        | Python 3.12                |
| Architecture   | x86_64                     |
| Execution Role | aws-resource-notifier-role |

---

# Create Function

Click:

```text
Create Function
```

AWS creates the Lambda function.

---

# Verify Function Creation

You should see:

```text
Function Overview
```

with:

```text
Function Name:
aws-resource-notifier
```

Status:

```text
Active
```

---

# Configure Environment Variable

Open:

```text
Configuration
→ Environment Variables
```

Click:

```text
Edit
```

---

# Add Environment Variable

Click:

```text
Add Environment Variable
```

---

## Key

Enter:

```text
SNS_TOPIC_ARN
```

---

## Value

Paste your SNS Topic ARN.

Example:

```text
arn:aws:sns:us-east-1:123456789012:aws-resource-alerts
```

Your value will be different.

Copy ARN from:

```text
SNS
→ Topics
→ aws-resource-alerts
```

---

# Save Environment Variable

Click:

```text
Save
```

Verify:

| Key           | Value         |
| ------------- | ------------- |
| SNS_TOPIC_ARN | SNS Topic ARN |

---

# Open Code Editor

Navigate:

```text
Lambda
→ aws-resource-notifier
→ Code
```

---

# Remove Default Code

Delete everything in:

```python
lambda_function.py
```

---

# Add Lambda Code

Paste:

```python
```python
import boto3
import os

sns = boto3.client('sns')

TOPIC_ARN = os.environ['SNS_TOPIC_ARN']

def lambda_handler(event, context):

    detail = event.get('detail', {})

    event_name = detail.get('eventName', 'Unknown')
    service = detail.get('eventSource', 'Unknown')
    region = detail.get('awsRegion', 'Unknown')
    event_time = detail.get('eventTime', 'Unknown')

    action = "Modified"

    if event_name.startswith("Create") or event_name == "RunInstances":
        action = "Created"

    elif event_name.startswith("Delete") or event_name == "TerminateInstances":
        action = "Deleted"

    message = f"""
AWS Resource Alert

Action : {action}
Service : {service}
Event : {event_name}
Region : {region}
Time : {event_time}
"""

    sns.publish(
        TopicArn=TOPIC_ARN,
        Subject='AWS Resource Alert',
        Message=message
    )

    return {
        'statusCode': 200
    }
```

```

---

# Code Explanation

## Import Libraries

```python
import json
import boto3
import os
```

Used for:

* AWS SDK
* Environment Variables
* JSON Processing

---

## SNS Client

```python
sns = boto3.client('sns')
```

Creates SNS connection.

---

## Read Topic ARN

```python
TOPIC_ARN = os.environ['SNS_TOPIC_ARN']
```

Reads ARN from Environment Variables.

---

## Receive Event

```python
def lambda_handler(event, context):
```

Entry point for Lambda execution.

---

## Extract Event Details

```python
detail = event.get('detail', {})
```

Reads CloudTrail event information.

---

## Detect Action Type

```python
Created
Deleted
Modified
```

Determines action performed.

---

## Build Email Message

Creates user-friendly notification.

Example:

```text
AWS Resource Notification

Action  : Created
Service : ec2.amazonaws.com
Event   : RunInstances
Region  : us-east-1
Time    : 2026-06-14T10:00:00Z
```

---

## Publish Message

```python
sns.publish(...)
```

Sends email through SNS.

---

# Deploy Code

Click:

```text
Deploy
```

Wait for:

```text
Successfully Updated
```

---

# Verify Deployment

Check:

```text
Last Modified
```

Updated timestamp should appear.

---

# Test Lambda Manually

Click:

```text
Test
```

---

## Create Test Event

Event Name:

```text
test-event
```

Sample Event:

```json
{
  "detail": {
    "eventName": "CreateBucket",
    "eventSource": "s3.amazonaws.com",
    "awsRegion": "us-east-1",
    "eventTime": "2026-06-14T10:00:00Z"
  }
}
```

---

# Run Test

Click:

```text
Test
```

Expected Result:

```text
Status: Succeeded
```

---

# Verify Email

Check inbox.

Expected email:

```text
AWS Resource Notification

Action  : Created
Service : s3.amazonaws.com
Event   : CreateBucket
Region  : us-east-1
Time    : 2026-06-14T10:00:00Z
```

---

# Verify CloudWatch Logs

Open:

```text
Monitor
→ View CloudWatch Logs
```

Verify:

```text
No Errors
```

---

# Validation Checklist

```text
✓ Lambda Opened
✓ Function Created
✓ Runtime Selected
✓ IAM Role Selected
✓ Environment Variable Added
✓ Lambda Code Added
✓ Code Deployed
✓ Manual Test Passed
✓ Email Received
✓ CloudWatch Logs Verified
```

---

# Architecture After Step 5

```text
SNS
   ↑
Lambda
```

Lambda is now ready to receive EventBridge events.

---

# Expected Output

Function:

```text
aws-resource-notifier
```

Runtime:

```text
Python 3.12
```

Environment Variable:

```text
SNS_TOPIC_ARN
```

Status:

```text
Active
```

---
