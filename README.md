# Agentic AI AP Employee

An Autonomous AI Account Payable (AP) System that leverages multiple specialized agents to streamline invoice processing, validation, matching, compliance checks, approvals, and payments.

## Overview

This system replaces manual AP workflows with an agentic AI architecture that autonomously handles the entire invoice-to-payment lifecycle while maintaining compliance, audit trails, and human oversight where needed.

## Architecture

### High-Level System Design

![High Level Architecture](public/architechture.png)


```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              Invoice Sources                                │
│          (Manual PDF Upload | Gmail | Outlook)                              │
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           Invoice Ingestion                                 │
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                    The Orchestrator (State Machine)                         │
│         Manages Worker Agents | Enforces Guardrails | Maintains Audit Logs  │
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
        ┌─────────────────────────┼─────────────────────────┐
        ▼                         ▼                         ▼
┌───────────────┐    ┌────────────────────┐    ┌───────────────────┐
│   Policies    │    │   Worker Agents    │    │ Tools/Integration │
├───────────────┤    ├────────────────────┤    ├───────────────────┤
│ • Approval    │    │ • Document Agent   │    │ • Xero            │
│ • Compliance  │◄──►│ • Validation Agent │◄──►│ • QuickBooks      │
│ • SLA         │    │ • Matching Agent   │    │ • Slack           │
└───────────────┘    │ • Compliance Agent │    │ • Email APIs      │
                     │ • Approval Agent   │    │ • Banking APIs    │
                     │ • Payment Agent    │    └───────────────────┘
                     │ • Communication    │
                     └─────────┬──────────┘
                               │
                               ▼
                     ┌────────────────────┐
                     │    Memory / DB     │
                     ├────────────────────┤
                     │ • Relational DB    │
                     │ • Vector DB        │
                     └────────────────────┘
```



### Worker Agents

| Agent | Responsibility |
|-------|----------------|
| **Invoice/Document Agent** | Extracts and parses invoice data from various formats (PDF, images, emails) |
| **Validation Agent** | Validates invoice data, checks for duplicates, verifies vendor information |
| **Matching Agent** | Performs 3-way matching between invoices, POs, and receipts |
| **Compliance/Policy Agent** | Ensures invoices meet compliance requirements and internal policies |
| **Approval Agent** | Routes invoices through appropriate approval workflows based on rules |
| **Payment Agent** | Processes approved payments and updates payment records |
| **Communication Agent** | Handles communications with vendors, approvers |

## Database Schema
![Database Modelling](public/db-modelling.png)

### Entity Descriptions

| Table | Purpose |
|-------|---------|
| `vendors` | Stores vendor information |
| `invoices` | Core invoice data with amounts, dates, and vendor references |
| `line_items` | Individual line items within each invoice |
| `vendor_bank_accounts` | Bank account details for vendor payments |
| `purchase_orders` | Stores Purchase orders details  |
| `receipts` | Goods receipt records for matching verification |
| `approval_states` | Tracks approval workflow states and durations |
| `audits` | Complete audit trail of all state changes |

## Features

- **Multi-Source Invoice Ingestion**: Accept invoices via PDF upload, Gmail, and Outlook
- **Document Processing**: Extraction of invoice data from various formats and channels
- **Automated Validation**: Duplicate detection, vendor verification, and data validation
- **Smart Matching**: 3-way matching between invoices, POs, and receipts
- **Policy Enforcement**: Configurable approval policies, compliance rules, and SLA tracking
- **Approval Workflows**: Dynamic routing based on amount thresholds and business rules
- **Complete Audit Trail**: Full traceability of all actions and state changes
- **Semantic Memory**: Vector database for fraud detection and learning from exceptions


