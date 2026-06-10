# Lab Inventory & Refill Alert System — Biotech Lab Automation

> Smart inventory monitoring with automatic LOW/CRITICAL detection and refill alerts.



![Status](https://img.shields.io/badge/Status-Complete-brightgreen)



---

## What It Does

Automatically monitors lab inventory levels in Airtable, detects LOW and CRITICAL stock, and sends formatted Gmail alerts for refilling.

- **Auto status detection** — GOOD / LOW / CRITICAL calculated automatically
- **Refill alerts** — Gmail notification when stock is low
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
Airtable fetches all inventory items
        ↓
Formula field calculates status per item
        ↓
Filter: LOW or CRITICAL items only
        ↓
Loop Over Items — process each alert
        ↓
Gmail sends refill alert email
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
| Last Alerted | Date |

---

## Status Formula

```
IF({Current Stock}=0, "CRITICAL",
  IF({Current Stock}<={Minimum Threshold}*0.5, "CRITICAL",
    IF({Current Stock}<{Minimum Threshold}, "LOW", "GOOD")))
```

---

## Key Features

- Automatic GOOD / LOW / CRITICAL status detection
- Multi-item inventory tracking
- Loop Over Items for reliable multi-record processing
- Airtable filter: only LOW and CRITICAL items alerted
- Gmail HTML formatted refill alerts

---

## Technical Notes

- Status field: Formula type in Airtable (not Single Select)
- Loop Over Items node added for correct multi-item processing
- Airtable filter: `OR({Status}='LOW', {Status}='CRITICAL')`

---

*Built by Deman Meshram | BSc Biotechnology + AI Automation*
