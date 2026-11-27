# Installation and Setup Guide

## 1. Overview

This guide explains how to configure and test the AI Marketing & Sales Engine using the sanitized n8n workflow provided in this repository.

The current version is a working prototype, not a production-ready application.

## 2. Prerequisites

The following accounts and services are required:

| Platform | Purpose |
|---|---|
| Clay | Prospect research and enrichment |
| n8n | Workflow orchestration |
| HubSpot | CRM and contact management |
| Gmail | Email draft creation and delivery |
| Slack | Human approval and notifications |

Calendly will be introduced in a future version.

Platform costs, usage limits and feature availability depend on the selected subscription plans.

## 3. Import the n8n Workflow

1. Download the workflow file from this repository:
   [prospect-intake.json](../workflows/prospect-intake.json)
2. Open your n8n workspace.
3. Create a new workflow.
4. Open the workflow menu.
5. Select Import from File.
6. Upload the downloaded JSON file.
7. Save the imported workflow.

The workflow is inactive by default.

Do not activate it until credentials, approval permissions and testing have been completed.

## 4. Configure Authentication

The repository does not include original account credentials.

Create and configure the following credentials in your own n8n environment:

| Credential | Integration |
|---|---|
| Header Authentication | Incoming Clay webhook |
| HubSpot OAuth2 | CRM operations |
| Gmail OAuth2 | Draft creation and email delivery |
| Slack API | Approval requests |

Assign the appropriate credentials to every relevant workflow node.

Never store API keys, access tokens, passwords or authentication headers in GitHub.

## 5. Configure Clay

Create a prospect research table containing:

- First Name
- Last Name
- Work Email
- Company Name
- Job Title
- LinkedIn Profile
- Final Email Subject
- Final Email Draft
- Outreach Status

Configure company discovery, decision-maker identification, enrichment and AI-assisted email personalization according to your ideal customer profile.

Add an HTTP API action to send qualified prospect records to the n8n webhook.

### HTTP Configuration

- Method: POST
- URL: Your n8n webhook URL
- Content-Type: application/json
- Authentication: Matching webhook header credentials

### Example Payload

```json
{
  "first_name": "Alex",
  "last_name": "Example",
  "email": "alex@example.com",
  "company_name": "Example AI Company",
  "job_title": "Head of Marketing",
  "linkedin_url": "",
  "email_subject": "Workflow integration test",
  "email_body": "This is a controlled test message.",
  "outreach_status": "Draft"
}
```

The example contains fictional prospect information.

Replace the recipient address with a test mailbox you control before executing the workflow.

Map the JSON fields dynamically to the corresponding Clay columns.

Keep automatic execution disabled during initial testing.

## 6. Configure HubSpot

Connect your HubSpot account using OAuth2.

Assign the credential to the following nodes:

- HubSpot — Upsert Contact
- HubSpot — Outreach Sent
- HubSpot — Outreach Rejected

Verify the contact property mappings:

- Email
- First name
- Last name
- Company name
- Job title
- LinkedIn URL
- Lead status

The workflow uses the email address to identify and upsert contacts.

### Lead Status Mapping

| Outcome | HubSpot Lead Status |
|---|---|
| Email approved and Gmail API request successful | ATTEMPTED_TO_CONTACT |
| Email rejected | OPEN |

Verify that these lead status values are available in your HubSpot configuration.

## 7. Configure Gmail

Connect your Gmail account using OAuth2.

Assign the credential to:

- Gmail — Create Outreach Draft
- Gmail — Send Approved Email

The first node creates an email draft using the subject, body and recipient received from Clay.

The second node sends the existing draft through the Gmail API.

### Gmail API Configuration

Method:

POST

Endpoint:

https://gmail.googleapis.com/gmail/v1/users/me/drafts/send

Authentication:

Predefined Gmail OAuth2 credential

The request body contains the draft ID returned by the Gmail draft creation node.

Ensure that the credential has the necessary Gmail permissions and that its HTTP request domain restrictions permit gmail.googleapis.com.

## 8. Configure Slack Approval

Create a dedicated private Slack channel, for example:

`#outreach-approvals`

Configure a Slack app and connect it to n8n with the permissions required by the approval node.

Ensure that the app can access the selected channel.

Configure interactive approval functionality according to your n8n and Slack environment.

In the Slack — Human Approval node:

1. Select your own Slack credential.
2. Replace the channel placeholder with your channel ID.
3. Configure the Send and Wait for Response operation.
4. Enable responder identification.
5. Replace the approver placeholder with an authorized Slack user ID.
6. Configure the approval buttons.

Approval buttons:

- Approve & Send
- Reject

Where supported, enable approval outcome display and remove the buttons after a decision.

Do not allow unrestricted approval access.

Verify that the workflow cannot send an email without an authorized approval.

## 9. Controlled Testing

Use fictional prospect records and an email address you control.

### Test A — Prospect Validation

- Submit a record with missing required fields.
- Submit a record with an invalid email format.
- Submit a record with an outreach status other than Draft.

Expected result:

Invalid records do not proceed to CRM processing or email draft creation.

### Test B — Approval Branch

1. Start the workflow in test mode.
2. Submit a valid test payload to the test webhook.
3. Verify contact creation in HubSpot.
4. Verify draft creation in Gmail.
5. Review the Slack approval request.
6. Select Approve & Send.
7. Verify email receipt in the controlled mailbox.
8. Verify the updated HubSpot lead status.

Expected result:

The approved draft is sent and HubSpot records the outreach attempt.

### Test C — Rejection Branch

1. Submit a separate valid test record.
2. Wait for the Slack approval request.
3. Select Reject.
4. Verify that no email is sent.
5. Verify that HubSpot records the OPEN lead status.

Expected result:

The email is not sent by the workflow.

The rejected draft remains in Gmail and requires manual cleanup.

### Test D — Approval Security

- Verify that unauthorized users cannot approve outreach.
- Verify that an unanswered approval request does not send an email.
- Verify that only the approved draft is sent.
- Restrict access to the approval channel and Gmail account.

## 10. Production Readiness Checklist

Complete the following before enabling live outreach:

- [ ] Configure all credentials
- [ ] Test the imported workflow
- [ ] Verify dynamic Clay field mappings
- [ ] Verify webhook authentication
- [ ] Verify restricted Slack approval
- [ ] Test both approval and rejection branches
- [ ] Implement duplicate outreach prevention
- [ ] Implement suppression and opt-out management
- [ ] Verify applicable outreach and privacy requirements
- [ ] Improve and test email deliverability
- [ ] Configure error handling and monitoring
- [ ] Define data retention policies
- [ ] Verify production webhook configuration

Only publish and enable the production workflow after the required safeguards and tests have been completed.

## 11. Troubleshooting

| Issue | Suggested check |
|---|---|
| Webhook authentication fails | Verify the authentication header and credential |
| Prospect validation fails | Check required fields and Draft status |
| HubSpot contact is missing | Verify OAuth permissions and field mappings |
| Gmail draft is not created | Check Gmail credentials and recipient mapping |
| Slack approval is not received | Verify app permissions and channel access |
| Approval buttons do not respond | Review Slack interactive configuration |
| Gmail API request fails | Verify OAuth permissions, draft ID and allowed domains |
| Email arrives in spam | Review sender reputation, authentication and message quality |
| Duplicate drafts appear | Implement idempotency and duplicate outreach checks |

## 12. Current Status

The original workflow has successfully completed controlled approval and rejection tests.

The sanitized repository version has not yet been independently reimported and tested.

Production deployment, duplicate outreach prevention, deliverability improvements and Calendly integration remain under development.

For additional technical details, see:

[Technical Architecture](architecture.md)

---

Designed and developed by Uliana.

Fractional CMO & Marketing Engineer
