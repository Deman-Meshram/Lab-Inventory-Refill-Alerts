# Lab Inventory & Refill - Expiry Alert System — Biotech Lab Automation

## 🎬 Demo Video
[▶ Watch Demo on YouTube](https://youtu.be/S0Wpnj3XEz4)

---

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

## How It Works For The User

**Lab manager do only 2 things:**
1. Current Stock number update karo jab bhi restock ho
2. Expiry Date daalo jab naya reagent aaye

**Everything else is fully automatic:**
- Stock Status — GOOD / LOW / CRITICAL — formula automatically calculate karta hai
- Expiry Status — VALID / EXPIRING SOON / EXPIRED — formula automatically detect karta hai
- Alert email — scheduled trigger pe automatically aata hai
- Reorder list — automatically banta hai critical items se
- Expiry alerts — automatically detect aur email mein include hote hain

**Zero configuration needed after setup — just update stock numbers and dates, everything else is automatic.**
Formula fields calculate Stock Status + Expiry Status automatically
        ↓
Loop Over Items — process each alert
        ↓
JavaScript builds HTML with summary + table + alerts
        ↓
Gmail sends formatted refill + expiry alert
```

---

## Airtable Schema

| Field | Type | Updated By |
|---|---|---|
| Item Name | Single line text | Manual (once) |
| Current Stock | Number | Manual (on restock) |
| Minimum Threshold | Number | Manual (once) |
| Unit | Single select | Manual (once) |
| Supplier | Single line text | Manual (once) |
| Status | Formula (auto) | ✅ Automatic |
| Expiry Date | Date | Manual (on restock) |
| Expiry Status | Formula (auto) | ✅ Automatic |
| Last Alerted | Date | ✅ Automatic |

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
- Expiry Status field: Formula type in Airtable — updates automatically every day
- Airtable filter: `OR({Status}='LOW', {Status}='CRITICAL', {Expiry Status}='EXPIRED', {Expiry Status}='EXPIRING SOON')`
- Loop Over Items node for correct multi-item processing
- JavaScript node builds HTML with summary + reorder list + expiry alerts

---

*Built by Deman Meshram | BSc Biotechnology + AI Automation*
