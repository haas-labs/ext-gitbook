---
description: >
  Monitors the on-chain balance of each tokenomics allocation address and emits
  a daily report per allocation category, sourcing allocation definitions from
  the Extractor API and live transfer data from Dune Analytics.
---

# Total Supply Monitor by Allocations

## Overview

The Total Supply Monitor by Allocations tracks the circulating balance of individual **tokenomics allocation addresses** (e.g., team wallet, treasury, vesting contracts). Rather than comparing against a single expected cap, it reports the real-time balance of every active allocation defined in the project's tokenomics configuration.

On each scheduled run the monitor:

1. Fetches active allocation categories for the monitored contract from the **Extractor API**
2. Queries **Dune Analytics** for per-address transfer totals (`debit`, `credit`, `balance`) on-chain
3. Emits one alert per allocation with the current balance

---

## Allocation Source

Allocation definitions are pulled automatically from the **Extractor API**.

---

## Decimals

The number of decimal places used by the token contract. Passed directly to the Dune Analytics query to return human-readable balance values.

| Field | Type | Default |
|-------|------|---------|
| `decimals` | `int` | `18` |

Override this for tokens that use non-standard precision (e.g., USDC uses `6`).

---

## Severity

A fixed severity level applied to **all** alerts emitted by this monitor. Unlike the basic Total Supply Monitor, there is no match/mismatch distinction — every run unconditionally emits one alert per allocation.

| Field | Type | Default |
|-------|------|---------|
| `severity` | `float` | `0.1` (Info) |

Set a higher severity if you want allocation balance updates to appear as actionable alerts rather than informational reports.

---

## Alert Metadata

Every alert emitted by this monitor includes the following fields:

| Field | Description |
|-------|-------------|
| `monitored_contract` | Address of the token contract being monitored |
| `allocation` | Human-readable name of the allocation category (from Extractor API) |
| `real_value` | Current on-chain balance of the allocation address (adjusted for decimals) |

---

## Alert Types

| Event Type | Trigger |
|---|---|
| `total_supply_by_allocations` | Emitted once per active allocation per scheduled run |
