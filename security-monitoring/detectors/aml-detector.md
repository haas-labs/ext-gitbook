---
description: >
  Monitors blockchain transactions and flags addresses that appear on Anti-Money
  Laundering (AML) lists, including exploit and sanctions databases.
---

# AML Detector

## Overview

The AML Detector monitors blockchain transactions and flags addresses that appear on AML-related lists, including sanctions and exploit databases.

It functions as a **pre-screening tool** — providing fast initial risk detection before deeper analysis.

> ⚠️ AML scores are not always real-time. When you open an alert, the system may re-query or recalculate the score. As a result, the score shown in the alert may differ slightly from the one that originally triggered it.

If an address has **no score data available** (depending on the selected data source), the alert defaults to **Info** severity instead of being skipped.

---

## Source

Specifies which address list the detector checks against.  
Only **one source** can be selected at a time.

| Value | Meaning |
|-------|---------|
| `ext` | Extractor’s proprietary AML address database |
| `custom` | Only addresses defined in the **Custom Addresses** field |
| `empty` | Global internal address list |

---

## Tags Filter

Allows you to narrow alerts to specific AML categories.

- Enter tags as **comma-separated values**  
  Example: `sanctions, exploit`
- Prefix a tag with `!` to **exclude** it  
  Example: `!sanctions`

| Tag | Meaning |
|------|---------|
| `cybercrime` | OFAC-listed or other cybercrime-related activity |
| `sanctions` | Directly sanctioned entities |
| `sanctions_exposure` | Indirect exposure to sanctioned entities |
| `suspicious` | Flagged for suspicious behavior |
| `exploit` | Involved in a known exploit |

---

## Score Condition

Filters alerts based on the AML risk score range: **0.0 – 100.0**

⚠️ Important behavior:

- Using `> 0` → addresses **without a score will NOT trigger**
- Using `>= 0` → addresses **without a score WILL trigger**

Choose carefully depending on whether you want to include unscored addresses.

---

## Tracking Options

Select which transaction components should be evaluated:

- **Track Transaction** — checks all addresses involved in the transaction
- **Track From** — checks the sender address
- **Track To** — checks the recipient address
- **Track Token** — checks token transfer addresses  
  - The monitored address is treated as the **token contract address**

---

## Custom Addresses

Add your own addresses to monitor in **CSV format**:

```
Address, Score, Name, Tags
0x1049a94a2238297156826cfcd8b35a3c0400ee98, 80.0, Tornado Cash, ofac;sanctions
```


**Field Requirements:**

- **Score** — value between `0.0` and `100.0`
- **Tags** — semicolon-separated (e.g., `ofac;sanctions`)

---

## Exclude Addresses

Addresses listed here will be ignored by the detector — even if they appear in **Custom Addresses**.

- Enter one address per line  
  **or**
- Provide a comma-separated list

No additional metadata is required.

---

## Severity

If set to **Auto**, the detector automatically maps the AML score to a severity level.

You may override this by selecting a fixed severity:

- **Critical**
- **High**
- **Medium**
- **Low**
- **Info**
