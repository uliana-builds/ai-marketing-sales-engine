
# AI Marketing & Sales Engine

**Marketing Engineering | Project 01**

An AI-powered lead generation and sales automation system built with Clay, n8n, HubSpot, Gmail and Slack.

**Automated research. Personalized outreach. Human control.**

## Project Overview

This project explores how AI, data enrichment and workflow automation can be combined to build a scalable B2B lead generation and outreach system.

The engine is designed to reduce manual prospect research, automate CRM management and generate personalized outreach while maintaining human oversight of external communications.

## Business Objectives

- Identify companies matching a defined Ideal Customer Profile (ICP).
- Find and enrich relevant decision-makers.
- Generate personalized, research-driven outreach.
- Reduce repetitive manual marketing tasks.
- Centralize prospect management in a CRM.
- Maintain human control over outbound communications.
- Build reusable and scalable marketing infrastructure.

## Technology Stack

| Technology | Function |
|---|---|
| Clay | Prospect discovery, data enrichment and AI research |
| n8n | Workflow orchestration and automation |
| JavaScript | Data normalization and validation |
| HubSpot | CRM and lead status management |
| Gmail | Email draft creation and delivery |
| Slack | Human approval and notifications |
| Calendly | Meeting booking integration (planned) |

## System Architecture

```
Clay - Prospect Discovery & Enrichment
                 |
                 v
n8n - Prospect Intake
                 |
                 v
Normalize Prospect Data
                 |
                 v
Validate Prospect
                 |
                 v
HubSpot - Upsert Contact
                 |
                 v
Gmail - Create Outreach Draft
                 |
                 v
Slack - Human Approval
                 |
                 v
         Approval Decision
          /            \
     Approved        Rejected
        |                |
        v                v
  Gmail - Send      No Email Sent
        |                |
        v                v
  HubSpot -         HubSpot -
  Outreach Sent     Outreach Rejected
```

## Workflow Implementation

### 1. Prospect Discovery and Enrichment

Clay identifies and enriches companies and decision-makers based on predefined targeting criteria.

The prospecting process includes:

- Company and industry research
- Decision-maker identification
- Contact information enrichment
- AI-powered opportunity analysis
- Personalized email subject lines
- Personalized outreach drafts

### 2. Data Processing

n8n receives prospect information through a webhook.

Incoming data is normalized and validated before being transferred to downstream systems.

The prospect data structure includes:

- First name
- Last name
- Work email
- Company name
- Job title
- LinkedIn profile
- Personalized email subject
- Personalized email body
- Outreach status

### 3. CRM Integration

HubSpot acts as the central source of truth for prospect management.

The workflow creates or updates contact records and maintains outreach-related lead statuses.

Contact deduplication helps prevent multiple CRM records for the same prospect.

### 4. Personalized Email Generation

Personalized outreach content generated in Clay is transferred to Gmail, where an email draft is created automatically.

The workflow requires human approval before sending an email.

### 5. Human-in-the-Loop Approval

Slack delivers an approval request containing the proposed outreach.

Authorized users can approve or reject the email.

**Approved:**

- Send the approved email through Gmail.
- Update the HubSpot lead status to Attempted to contact.

**Rejected:**

- Do not send the email.
- Keep the contact open for further review.

## Testing and Validation

| Test Scenario | Result |
|---|---|
| Webhook intake with test data | Passed |
| Prospect data normalization | Passed |
| HubSpot contact creation | Passed |
| HubSpot contact deduplication | Passed |
| Gmail draft creation | Passed |
| Slack approval request | Passed |
| Authorized user approval | Passed |
| Approved email delivery | Passed |
| HubSpot status update | Passed |
| Rejected email prevention | Passed |
| Rejected contact status | Passed |
| Dynamic Clay payload validation | Pending |
| Duplicate draft prevention | Pending |
| Email deliverability | Requires improvement |
| Production deployment | Pending |

Both approval and rejection branches have been successfully tested using controlled test data.

An approved test email was received but classified as spam. Deliverability improvements are required before production outreach.

## Security and Responsible Automation

The system is designed around human oversight and responsible outreach.

Implemented controls:

- Human approval before outbound email delivery.
- Restricted approval permissions.
- Data validation before downstream processing.

Additional requirements before public release and production deployment:

- Remove credentials and private identifiers from exported workflow files.
- Exclude real prospect information from public documentation.
- Implement outreach eligibility and compliance checks.
- Implement duplicate draft prevention.
- Improve email deliverability.

## Expected Business Impact

The system is designed to:

- Reduce time spent on manual prospect research.
- Improve consistency of personalized outreach.
- Reduce repetitive CRM administration.
- Improve visibility into outreach activity.
- Maintain human oversight of AI-generated communications.
- Support scalable client acquisition.

Quantitative performance metrics will be collected following production deployment.

## Development Roadmap

### Phase 1 - Core Lead Generation Engine

- [x] Prospect discovery and enrichment
- [x] AI-powered outreach personalization
- [x] n8n workflow orchestration
- [x] HubSpot integration
- [x] Gmail draft creation
- [x] Slack human approval
- [x] Approval and rejection branch testing
- [ ] Dynamic Clay payload testing
- [ ] Duplicate draft prevention
- [ ] Email deliverability optimization
- [ ] Outreach compliance checks
- [ ] Production deployment

### Phase 2 - Sales Automation

- [ ] Automated follow-up management
- [ ] Reply tracking
- [ ] Calendly integration
- [ ] Meeting notifications
- [ ] Automated CRM status updates for booked meetings

### Phase 3 - Analytics and Optimization

- [ ] Daily and weekly performance reports
- [ ] Outreach conversion tracking
- [ ] Lead scoring optimization
- [ ] Marketing and sales performance analytics

## Project Status

**Version:** 0.1 - Working Prototype

**Started:** November 2026

**Status:** Core workflow tested; production readiness in progress.

## About the Author

Built by Uliana - Fractional CMO & Marketing Engineer, MSc Business Analytics & AI student.

I work at the intersection of marketing, technology and business strategy, combining AI, automation and data analytics to build smarter, more scalable growth systems.

This project is part of my Marketing Engineering portfolio, focused on solving real-world business challenges through marketing strategy, artificial intelligence, business analytics and workflow automation.

---

**Marketing Engineering | AI | Automation | Growth | Business Analytics**
