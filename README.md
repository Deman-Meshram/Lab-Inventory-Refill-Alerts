# Lab Inventory & Refill - Expiry Alert System — Biotech Lab Automation

> Smart inventory monitoring with automatic LOW/CRITICAL detection, refill alerts, and expiry tracking.



![Status](https://img.shields.io/badge/Status-Complete-brightgreen)



---

## The Problem

Lab managers manually check inventory levels and expiry dates — a tedious, error-prone process. Running out of critical reagents or using expired chemicals mid-experiment can halt research for days and cost thousands.

Most labs don't know they're out of stock or using expired reagents until it's too late.

---

## The Solution

Lab Inventory & Refill Alert automatically monitors all inventory levels and expiry dates in Airtable, detects LOW/CRITICAL stock and EXPIRED/EXPIRING SOON reagents, and delivers a complete formatted email alert — so lab managers always stay ahead.

- Zero manual checking — runs on schedule automatically
- Instant CRITICAL/LOW stock detection via smart formula
- Expiry tracking — EXPIRED and EXPIRING SOON alerts
- Color-coded email with full inventory + expiry status
- Reorder list + Expiry alerts in every email

---

## Time & Effort Saved

| Task | Manual | This Automation |
|---|---|---|
| Checking stock levels | Daily manual check | Zero — fully automatic |
| Identifying critical items | Visual scanning | Auto-detected instantly |
| Checking expiry dates | Manual date checking | Auto-detected instantly |
| Notifying team | Manual email | Auto-delivered to Gmail |
| Tracking reorder needs | Sticky notes/memory | Clear reorder list in email |

---

## Final Output

- ✅ Inventory Summary — Total / Critical / Low / Expired / Expiring Soon
- ✅ Color-coded table — Stock Status + Expiry Status per item
- ✅ Immediate Reorder List — critical stock items
- ✅ Expiry Alerts — expired and expiring soon items with dates
- ✅ Supplier name per item for quick reordering
- ✅ Professional HTML Gmail alert

---

## What It Does

Automatically monitors lab inventory levels and expiry dates in Airtable, detects issues, and sends formatted Gmail alerts.

- **Auto status detection** — GOOD / LOW / CRITICAL calculated automatically
- **Expiry tracking** — VALID / EXPIRING SOON / EXPIRED per item
- **Inventory Summary** — Total / Critical / Low / Expired / Expiring Soon count
- **Refill alerts** — Gmail notification when stock is low
- **Reorder list** — critical items highlighted separately for action
- **Expiry alerts** — expired and expiring soon items listed with dates
- **Multi-item monitoring** — tracks all lab items simultaneously
- **Airtable database** — full inventory management with history
- **Scheduled automation** — runs automatically, no manual trigger needed

---

## Tech Stack

| Layer | Tool |
|---|---|
| Automation | n8n workflow |
| Database | Airtable |
| Email | Gmail HTML |

---

## Architecture

```
Scheduled trigger (n8n)
        ↓
Airtable fetches LOW/CRITICAL + EXPIRED/EXPIRING SOON items
        ↓
Formula fields calculate Stock Status + Expiry Status
        ↓
Loop Over Items — process each alert
        ↓
JavaScript builds HTML with summary + table + alerts
        ↓
Gmail sends formatted refill + expiry alert
```

---

## Airtable Schema

| Field | Type |
|---|---|
| Item Name | Single line text |
| Current Stock | Number |
| Minimum Threshold | Number |
| Unit | Single select (L, mL, g, mg, pcs, box) |
| Supplier | Single line text |
| Status | Formula (auto) |
| Expiry Date | Date |
| Expiry Status | Formula (auto) |
| Last Alerted | Date |

---

## Status Formulas

**Stock Status:**
```
IF({Current Stock}=0, "CRITICAL",
  IF({Current Stock}<={Minimum Threshold}*0.5, "CRITICAL",
    IF({Current Stock}<{Minimum Threshold}, "LOW", "GOOD")))
```

**Expiry Status:**
```
IF({Expiry Date}="", "No Date",
  IF(DATETIME_DIFF({Expiry Date}, TODAY(), 'days') < 0, "EXPIRED",
    IF(DATETIME_DIFF({Expiry Date}, TODAY(), 'days') <= 30, "EXPIRING SOON",
      "VALID")))
```

---

## Technical Notes

- Status field: Formula type in Airtable (not Single Select)
- Expiry Status field: Formula type in Airtable
- Airtable filter: `OR({Status}='LOW', {Status}='CRITICAL', {Expiry Status}='EXPIRED', {Expiry Status}='EXPIRING SOON')`
- Loop Over Items node for correct multi-item processing
- JavaScript node builds HTML with summary + reorder list + expiry alerts

---

*Built by Deman Meshram | BSc Biotechnology + AI Automation*
