# Step 2: Create SNS Email Subscription

## Objective

Create an email subscription for the SNS Topic created in Step 1.

This allows SNS to send email notifications to your email address whenever Lambda publishes a message.

---

# What is an SNS Subscription?

An SNS Topic cannot directly send notifications until at least one subscriber is attached.

Example:

```text
SNS Topic
    ↓
Email Subscription
    ↓
your-email@example.com
```

Whenever SNS receives a message:

```text
Lambda
    ↓
SNS Topic
    ↓
Email Subscription
    ↓
Inbox
```

---

# Prerequisites

Before starting:

* SNS Topic must already exist.
* Topic status should be Active.
* You must have access to your email inbox.

Example Topic:

```text
aws-resource-alerts
```

---

# Open SNS Console

Login to AWS Console.

Search:

```text
SNS
```

Open:

```text
Amazon Simple Notification Service
```

---

# Navigate to Topics

From the left navigation menu:

```text
SNS
→ Topics
```

---

# Select Your Topic

Click:

```text
aws-resource-alerts
```

You will see Topic Details.

Example:

```text
Topic Name:
aws-resource-alerts

Type:
Standard

Status:
Active
```

---

# Create Subscription

Click:

```text
Create Subscription
```

Located near the top-right of the page.

---

# Subscription Details

You will see the Create Subscription page.

---

## Topic ARN

AWS automatically populates:

```text
arn:aws:sns:region:account-id:aws-resource-alerts
```

Example:

```text
arn:aws:sns:us-east-1:123456789012:aws-resource-alerts
```

Verify the ARN belongs to:

```text
aws-resource-alerts
```

Do not change it.

---

## Protocol

Open the Protocol dropdown.

Select:

```text
Email
```

Available options include:

```text
Email
Email-JSON
HTTP
HTTPS
Lambda
SMS
SQS
Application
Firehose
```

For this project choose:

```text
Email
```

---

## Endpoint

Enter your email address.

Example:

```text
sauravbagade7@gmail.com
```

or

```text
your-email@example.com
```

This email will receive all notifications.

Make sure:

* Email address is correct.
* You have access to the inbox.
* No spelling mistakes.

---

## Raw Message Delivery

Leave:

```text
Disabled
```

Default setting is recommended.

---

## Filter Policy

Leave empty.

Do not configure any filter policy.

Reason:

We want all notifications.

---

## Redrive Policy

Leave:

```text
None
```

No Dead Letter Queue required.

---

## Replay Policy

Leave default.

No changes required.

---

# Review Subscription

Verify:

| Setting              | Value                   |
| -------------------- | ----------------------- |
| Topic ARN            | aws-resource-alerts ARN |
| Protocol             | Email                   |
| Endpoint             | Your Email Address      |
| Raw Message Delivery | Disabled                |
| Filter Policy        | None                    |
| Redrive Policy       | None                    |

---

# Create Subscription

Click:

```text
Create Subscription
```

AWS will create the subscription.

---

# Subscription Status

Immediately after creation you will see:

```text
Pending Confirmation
```

Example:

```text
Subscription ID:
PendingConfirmation

Status:
Pending Confirmation
```

This is expected.

SNS will not send notifications until confirmation is completed.

---

# Check Your Email Inbox

Open:

```text
Gmail
Outlook
Yahoo Mail
Corporate Email
```

Search for:

```text
AWS Notification
```

or

```text
AWS SNS
```

or

```text
AWS Notification - Subscription Confirmation
```

---

# Example Confirmation Email

Subject:

```text
AWS Notification - Subscription Confirmation
```

Email body contains:

```text
You have chosen to subscribe to the topic:
aws-resource-alerts
```

You will also see:

```text
Confirm Subscription
```

or a confirmation URL.

---

# Confirm Subscription

Click:

```text
Confirm Subscription
```

AWS will open a browser page.

You should see:

```text
Subscription confirmed
```

or

```text
You have successfully subscribed
```

---

# Return to SNS

Go back to:

```text
SNS
→ Topics
→ aws-resource-alerts
→ Subscriptions
```

Refresh the page.

---

# Verify Subscription Status

The status should now be:

```text
Confirmed
```

Example:

| Endpoint                                                  | Protocol | Status    |
| --------------------------------------------------------- | -------- | --------- |
| [sauravbagade7@gmail.com](mailto:sauravbagade7@gmail.com) | Email    | Confirmed |

---

# If Confirmation Email Is Not Received

Wait:

```text
2-5 minutes
```

Then check:

* Inbox
* Spam Folder
* Promotions Tab
* Junk Folder

Search:

```text
AWS Notification
```

---

# Request Confirmation Again

If the email was deleted:

Select the subscription.

Click:

```text
Request Confirmation
```

AWS will send a new confirmation email.

---

# Test Email Delivery

Open:

```text
SNS
→ Topics
→ aws-resource-alerts
```

Copy Topic ARN.

Later, Lambda will publish to this topic.

When Lambda publishes:

```text
SNS
    ↓
Email
```

You should receive messages in your inbox.

---

# Common Issues

## Status Still Pending Confirmation

Reason:

Confirmation email was not clicked.

Solution:

Click:

```text
Confirm Subscription
```

from the email.

---

## Email Not Received

Check:

* Spam Folder
* Promotions Folder
* Correct Email Address

Request confirmation again.

---

## Wrong Email Address

Delete the subscription.

Create a new subscription with the correct email address.

---

# Expected Output

After successful completion:

```text
Topic:
aws-resource-alerts

Protocol:
Email

Endpoint:
your-email@example.com

Status:
Confirmed
```

---

# Validation Checklist

Verify all items below:

```text
✓ SNS Topic Created
✓ Subscription Created
✓ Protocol = Email
✓ Email Address Entered Correctly
✓ Confirmation Email Received
✓ Confirmation Link Clicked
✓ Status = Confirmed
```

---

# Architecture After Step 2

```text
SNS Topic
    ↓
Email Subscription
    ↓
Your Inbox
```

At this stage, SNS is ready to send email notifications.

---
