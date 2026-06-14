# Step 3: Create IAM Role for Lambda

## Objective

Create an IAM Role that allows the Lambda function to:

* Write logs to CloudWatch Logs
* Publish messages to SNS
* Be assumed by the Lambda service

This role will be attached to the Lambda function created in later steps.

---

# What is an IAM Role?

AWS Lambda cannot access AWS services directly.

Permissions must be granted using an IAM Role.

Example:

```text
Lambda
   ↓
IAM Role
   ↓
SNS
```

Without an IAM Role:

```text
Lambda → SNS
```

will fail with:

```text
AccessDenied
```

---

# Why Do We Need This Role?

This project requires Lambda to:

### Write Logs

```text
Lambda
   ↓
CloudWatch Logs
```

Required for troubleshooting.

---

### Send Notifications

```text
Lambda
   ↓
SNS
   ↓
Email
```

Required to send email alerts.

---

# Login to AWS Console

Sign in to AWS Console.

---

# Open IAM Service

Search:

```text
IAM
```

Open:

```text
Identity and Access Management (IAM)
```

---

# Navigate to Roles

From left navigation:

```text
IAM
→ Roles
```

---

# Create New Role

Click:

```text
Create Role
```

---

# Step 1: Select Trusted Entity

You will see:

```text
Trusted Entity Type
```

Choose:

```text
AWS Service
```

Reason:

The role will be used by an AWS service.

---

# Step 2: Select Service

Choose:

```text
Lambda
```

This tells AWS:

```text
This role will be assumed by Lambda
```

---

# Step 3: Select Use Case

Choose:

```text
Lambda
```

---

# Verify Selection

You should see:

| Setting             | Value       |
| ------------------- | ----------- |
| Trusted Entity Type | AWS Service |
| Service             | Lambda      |
| Use Case            | Lambda      |

---

# Click Next

Click:

```text
Next
```

---

# Add Permissions

AWS now asks:

```text
Add Permissions
```

---

# Search for Policy

Search:

```text
AWSLambdaBasicExecutionRole
```

---

# Select Policy

Check:

```text
AWSLambdaBasicExecutionRole
```

Description:

```text
Provides permissions for Lambda to write logs to CloudWatch.
```

---

# What Does This Policy Do?

Allows:

```text
logs:CreateLogGroup
logs:CreateLogStream
logs:PutLogEvents
```

Without this policy:

```text
Lambda Logs = Not Available
```

---

# Verify Selection

Selected Policies:

```text
AWSLambdaBasicExecutionRole
```

Only one policy should be selected.

---

# Click Next

Click:

```text
Next
```

---

# Name the Role

Role Name:

```text
aws-resource-notifier-role
```

---

# Role Description (Optional)

Example:

```text
IAM role used by Lambda to send SNS notifications and write CloudWatch logs.
```

---

# Review Configuration

Verify:

| Setting        | Value                       |
| -------------- | --------------------------- |
| Trusted Entity | AWS Service                 |
| Service        | Lambda                      |
| Use Case       | Lambda                      |
| Permission     | AWSLambdaBasicExecutionRole |
| Role Name      | aws-resource-notifier-role  |

---

# Create Role

Click:

```text
Create Role
```

AWS creates the role.

---

# Verify Role Creation

Open:

```text
IAM
→ Roles
→ aws-resource-notifier-role
```

You should see:

```text
Role Name:
aws-resource-notifier-role
```

---

# Verify Trust Relationship

Open:

```text
Trust Relationships
```

Verify:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Do not modify this policy.

---

# Verify Attached Permissions

Open:

```text
Permissions
```

You should see:

```text
AWSLambdaBasicExecutionRole
```

attached.

---

# Why This Role Is Not Enough Yet

Currently:

```text
Lambda
   ↓
CloudWatch Logs
```

works.

But:

```text
Lambda
   ↓
SNS
```

does NOT work yet.

Because SNS permissions are missing.

---

# Current Architecture

```text
Lambda
   ↓
IAM Role
   ↓
CloudWatch Logs
```

---

# Validation Checklist

Verify:

```text
✓ IAM Opened
✓ Roles Selected
✓ Create Role Clicked
✓ AWS Service Selected
✓ Lambda Selected
✓ AWSLambdaBasicExecutionRole Selected
✓ Role Name Entered
✓ Role Created
✓ Trust Relationship Verified
✓ CloudWatch Permissions Attached
```

---

# Common Mistakes

## Selecting Wrong Service

Wrong:

```text
EC2
```

Correct:

```text
Lambda
```

---

## Selecting AdministratorAccess

Wrong:

```text
AdministratorAccess
```

Correct:

```text
AWSLambdaBasicExecutionRole
```

Follow least privilege principle.

---

## Wrong Role Name

Use:

```text
aws-resource-notifier-role
```

to match documentation.

---

# Expected Output

You should now have:

```text
Role Name:
aws-resource-notifier-role

Attached Policy:
AWSLambdaBasicExecutionRole

Trust Relationship:
lambda.amazonaws.com
```

---

# Architecture After Step 3

```text
Lambda
   ↓
IAM Role
   ↓
CloudWatch Logs
```

---
