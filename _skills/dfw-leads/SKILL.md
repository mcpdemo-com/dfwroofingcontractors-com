---
name: dfw-leads
description: >
  Manages the lead workflow for dfwroofingcontractors.com. Handles logging
  incoming leads to the tracking sheet, updating lead status, and preparing
  lead data for contractor outreach. Use when: "log a new lead", "update lead
  status", "check the leads sheet", "mark lead as sold", "review pending leads",
  or "send leads to contractors". Always use this skill — do not improvise
  the lead workflow from memory.
---

# DFW Roofing Contractors — Lead Workflow Skill

Manages all lead tracking and routing for dfwroofingcontractors.com.

---

## Fixed Asset IDs — Update These Once Set Up

| Asset | Value |
|---|---|
| **Lead Sheet ID** | TBD — add once Google Sheet is created |
| **Lead Sheet Tab** | Leads |
| **Formspree Form ID** | TBD — add once Formspree form is created |
| **Contact Email** | robert@mcpdemo.com |

---

## Lead Sheet Column Reference

| Column | Notes |
|---|---|
| Lead ID | Auto: L001, L002, L003... |
| Date | ISO date — YYYY-MM-DD |
| Name | From form submission |
| Phone | From form submission |
| Email | From form submission |
| Address | Property address |
| Service | repair / replacement / storm / inspection / gutters / other |
| Message | Additional details from form |
| Status | New → Contacted → Sold → Passed → No Answer |
| Contractor | Name of contractor lead was sent to |
| Lead Value | $ amount if sold |
| Notes | Free text |

---

## Lead Status Definitions

| Status | Meaning |
|---|---|
| **New** | Just received — not yet acted on |
| **Contacted** | Reached out to the homeowner to qualify |
| **Sold** | Lead sold to a contractor |
| **Passed** | Sent to contractor free (relationship building) |
| **No Answer** | Could not reach homeowner after 3 attempts |
| **Bad Lead** | Invalid phone/email or out of service area |

---

## Workflow A — Log a New Lead Manually

Use when a lead comes in via email (Formspree) and needs to be logged.

```
Step 1 — Extract lead data from the Formspree email notification
  Fields: name, phone, email, address, service type, message, date

Step 2 — Generate Lead ID
  → MCPDEMO:read_google_sheet to get current last row
  → Increment: if last is L012, new is L013

Step 3 — Add row to sheet
  → MCPDEMO:add_row_to_sheet
     spreadsheetId: [Lead Sheet ID]
     sheetName: Leads
     row: { Lead ID, Date, Name, Phone, Email, Address, Service, Message, Status: "New" }

Step 4 — Confirm
  Tell user: "Lead [ID] logged — [Name], [Service], [City if in address]"
```

---

## Workflow B — Review New Leads

```
Step 1 — Read sheet
  → MCPDEMO:read_google_sheet
     spreadsheetId: [Lead Sheet ID]
     sheetName: Leads

Step 2 — Filter for Status = "New"
  List them as a summary table: Lead ID | Date | Name | Phone | Service

Step 3 — Ask user what action to take on each
```

---

## Workflow C — Mark Lead as Sold

```
Step 1 — Find the lead row
  → MCPDEMO:search_sheet
     lookupColumn: Lead ID
     lookupValue: [Lead ID]

Step 2 — Update status
  → MCPDEMO:update_sheet_row
     lookupColumn: Lead ID
     lookupValue: [Lead ID]
     updates:
       Status: Sold
       Contractor: [Contractor name]
       Lead Value: [$ amount]
       Notes: [any notes]

Step 3 — Confirm
  "Lead [ID] marked Sold → [Contractor] for $[amount]"
```

---

## Contractor Outreach Template

When sending a lead to a contractor via email:

```
Subject: New Roofing Lead — [Service Type] in [City/Area]

Hi [Contractor Name],

New lead from DFW Roofing Contractors:

Name: [Name]
Phone: [Phone]
Email: [Email]
Address: [Address]
Service Needed: [Service]
Details: [Message]
Submitted: [Date]

This lead is available at our standard rate of $[price].
Reply to claim it or call me at [phone].

— Robert
DFW Roofing Contractors
robert@mcpdemo.com
```

---

## Lead Pricing Guide (Starting Point)

| Service Type | Suggested Lead Price |
|---|---|
| Storm / Hail Damage | $100–200 |
| Full Replacement | $75–150 |
| Roof Repair | $50–100 |
| Inspection | $25–50 |
| Gutters | $25–50 |

Adjust based on: property size, specifics in message, contractor demand.
Storm damage leads during hail season command top of range.

---

## Rules

| Rule | Detail |
|---|---|
| **Log every lead** | Even bad ones — helps identify form spam patterns |
| **Respond within 24h** | Leads go cold fast — contact homeowner or sell same day |
| **One lead, one contractor** | Never sell the same lead twice |
| **Update status always** | Sheet must reflect real state at all times |
| **No PII in commit messages** | Never put names/phones in GitHub commits |
