# Step 4: Add SNS Publish Permission to IAM Role

## Objective

Grant permission to the Lambda execution role so it can publish messages to the SNS topic.

Without this step, Lambda will fail with:

```text
AuthorizationErrorException
AccessDeniedException
User is not authorized to perform: sns:Publish
```

This is one of the most important steps in the project.

---

# Why This Permission Is Required

Current Flow:

```text
Lambda
   ↓
SNS
   ↓
Email
```

When Lambda executes:

```python
sns.publish(...)
```

AWS checks whether the Lambda IAM Role has:

```text
sns:Publish
```

permission.

If permission is missing:

```text
Lambda = Failed
Email = Not Sent
```

---

# Open IAM

Login to AWS Console.

Search:

```text
IAM
```

Open:

```text
Identity and Access Management (IAM)
```

---

# Open Roles

Navigate:

```text
IAM
→ Roles
```

---

# Select Lambda Role

Open:

```text
aws-resource-notifier-role
```

or if AWS created a different role automatically:

```text
aws-resource-notifier-role-xxxxxxxx
```

Important:

Use the exact role attached to your Lambda function.

---

# Verify Current Permissions

You should currently see:

```text
AWSLambdaBasicExecutionRole
```

attached.

Example:

| Policy Name                 |
| --------------------------- |
| AWSLambdaBasicExecutionRole |

At this point Lambda can only write logs.

It still cannot publish to SNS.

---

# Create Inline Policy

Click:

```text
Add permissions
```

Select:

```text
Create inline policy
```

---

# Choose Policy Editor

Select:

```text
JSON
```

Delete any existing sample JSON.

---

# Paste SNS Policy

Paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SNSPublish",
      "Effect": "Allow",
      "Action": [
        "sns:Publish"
      ],
      "Resource": "*"
    }
  ]
}
```

---

# Policy Explanation

## Effect

```json
"Effect": "Allow"
```

Allows access.

---

## Action

```json
"sns:Publish"
```

Allows Lambda to publish messages.

Example:

```python
sns.publish(...)
```

---

## Resource

```json
"Resource": "*"
```

Allows publishing to all SNS topics.

---

# More Secure Version (Recommended)

Instead of:

```json
"Resource": "*"
```

Use your SNS Topic ARN:

Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SNSPublish",
      "Effect": "Allow",
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:us-east-1:123456789012:aws-resource-alerts"
    }
  ]
}
```

Replace:

```text
arn:aws:sns:us-east-1:123456789012:aws-resource-alerts
```

with your actual SNS Topic ARN.

This follows AWS security best practices.

---

# Validate Policy

Click:

```text
Next
```

AWS should show:

```text
No policy validation errors
```

---

# Policy Name

Enter:

```text
SNSPublishPolicy
```

---

# Policy Description (Optional)

Example:

```text
Allows Lambda to publish messages to SNS topic.
```

---

# Create Policy

Click:

```text
Create Policy
```

AWS attaches the inline policy to the role.

---

# Verify Policy Attachment

Open:

```text
IAM
→ Roles
→ aws-resource-notifier-role
```

Under Permissions you should see:

| Policy                      |
| --------------------------- |
| AWSLambdaBasicExecutionRole |
| SNSPublishPolicy            |

---

# Verify Lambda Role

Go to:

```text
Lambda
→ aws-resource-notifier
→ Configuration
→ Permissions
```

Verify the Execution Role matches:

```text
aws-resource-notifier-role
```

or the role you modified.

---

# Test Permission

After attaching the policy:

Lambda will be allowed to execute:

```python
sns.publish(
    TopicArn=TOPIC_ARN,
    Subject="AWS Resource Alert",
    Message=message
)
```

Successfully.

---

# Architecture After Step 4

```text
Lambda
   ↓
IAM Role
   ↓
SNS Publish Permission
   ↓
SNS Topic
   ↓
Email
```

---

# Validation Checklist

Verify:

```text
✓ IAM Opened
✓ Roles Selected
✓ Lambda Role Opened
✓ Create Inline Policy Clicked
✓ JSON Editor Selected
✓ SNS Policy Added
✓ Policy Name Entered
✓ Policy Created
✓ Policy Attached
✓ AWSLambdaBasicExecutionRole Present
✓ SNSPublishPolicy Present
```

---

# Common Errors

## AuthorizationErrorException

Reason:

```text
SNSPublishPolicy not attached
```

Solution:

Attach SNSPublishPolicy to the Lambda execution role.

---

## Wrong IAM Role Modified

Problem:

```text
Policy attached to different role
```

Solution:

Verify Lambda Execution Role:

```text
Lambda
→ Configuration
→ Permissions
```

Attach policy to that role.

---

## Incorrect SNS ARN

Problem:

```text
AccessDenied
```

Solution:

Use correct Topic ARN.

Example:

```text
arn:aws:sns:us-east-1:123456789012:aws-resource-alerts
```

---

# Expected Output

Role:

```text
aws-resource-notifier-role
```

Policies:

```text
AWSLambdaBasicExecutionRole
SNSPublishPolicy
```

Lambda can now:

```text
Write Logs
Publish SNS Messages
Send Email Notifications
```

---

# Architecture Status

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

IAM permissions are now fully configured.

---
