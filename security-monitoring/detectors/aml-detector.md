---
description: >-
  **AML Detector** monitors blockchain transactions and flags addresses that appear on Anti-Money Laundering (AML) lists, including exploit and sanctions lists.
---

# AML Detector

**Behavior**
When a transaction occurs, the detector checks all relevant addresses against known AML databases. Because AML scores are not always real-time, this detector acts as a **pre-screening tool** — think of it as a first pass that catches likely risks quickly.

When you open an alert, the system may re-query or recalculate the score, so the score shown in the alert might **differ slightly** from the one that triggered it.

If an address has **no score data available** (which can happen depending on the data source), the alert will default to **Info** severity rather than being skipped entirely.

**Source**
Specifies which address list the detector should check against. Only one source can be selected at a time.

| Value | Meaning |
|---|---|
| `ext` | Extractor’s proprietary AML address database |
| `custom` | Only addresses defined in the **Custom Addresses** field above |
| `empty` | Global internal address list |


**Tags Filter**  
Narrow alerts to specific AML categories. Enter tags **comma-separated**, e.g. `sanctions, exploit`. Prefix a tag with `!` to **exclude** it, e.g. `!sanctions`.

| Tag | Meaning |
|---|---|
| `cybercrime` | **OFAC-listed** or other cybercrime activity |
| `sanctions` | **Directly sanctioned** entities |
| `sanctions_exposure` | **Indirect exposure** to sanctioned entities |
| `suspicious` | Flagged for **suspicious behavior** |
| `exploit` | Involved in a **known exploit** |

**Score Condition**
Filters alerts based on the AML risk score (**0.0 – 100.0**). Keep in mind: if you set the condition to `> 0`, addresses with **no score on record will not be caught**. Use `>= 0` to include unscored addresses.

**Tracking Options**
- **Track Transaction** — checks addresses involved in the transaction itself
- **Track From** — checks the **sender** address
- **Track To** — checks the **recipient** address
- **Track Token** — checks addresses in token transfers; note that the monitored address is treated as the **token contract address**

**Custom Addresses**
Add your own addresses to monitor, in **CSV format**:

```
Address, Score, Name, Tags
0x1049a94a2238297156826cfcd8b35a3c0400ee98, 80.0, Tornado Cash, ofac;sanctions
```

- **Score**: 0.0 – 100.0
- **Tags**: semicolon-separated (e.g. `ofac;sanctions`)

**Exclude Addresses**
Addresses listed here will be **ignored** by the detector, even if they appear in your **Custom Addresses** list. Enter one address per line or comma-separated — **no extra metadata needed**.

**Severity**
If left on **Auto**, the detector automatically maps the AML score to a severity level. You can override this by selecting a fixed severity: **Critical**, **High**, **Medium**, **Low**, or **Info**.