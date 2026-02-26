---
description: >
  Monitors the total circulating supply of a token on a blockchain and alerts when
  the issued supply exceeds the expected threshold.
---

# Total Supply Monitor

## Overview

The Total Supply Monitor is a daily monitoring tool that tracks the total supply of a token contract on a blockchain network. It queries on-chain mint and burn event logs via **Dune Analytics** and compares the result against user-defined expected values.

An alert is emitted on every scheduled run:

- **Info** — the current issued supply is within the expected limit (no anomaly detected)
- **Warning / custom** — the current issued supply exceeds the expected issued amount (potential over-minting)

---

## Expected Total Supply

The hard cap or maximum authorised supply of the token, expressed as a **whole-unit integer** (i.e. already divided by token decimals).

| Field | Type | Required |
|-------|------|----------|
| `expected_total_supply` | `int` | Yes |

Example: for a token with a 1 billion cap and 18 decimals, set this to `1000000000`.

---

## Expected Total Issued

The maximum number of tokens that should currently be in circulation. The detector compares the **live on-chain issued supply** against this value.

| Field | Type | Required |
|-------|------|----------|
| `expected_total_issued` | `int` | Yes |

- If `current issued supply ≤ expected_total_issued` → **Info** alert (supply is within bounds)
- If `current issued supply > expected_total_issued` → **Warning** alert (supply exceeds expectation)

---

## Decimals

The number of decimal places used by the token contract. Used to convert raw on-chain values to human-readable units before comparison.

| Field | Type | Default |
|-------|------|---------|
| `decimals` | `int` | `18` |

Most ERC-20 tokens use 18 decimals. Override this for tokens that use a different precision (e.g. USDC uses `6`).

---

## Severity

Controls the severity level assigned to **mismatch** alerts (when the issued supply exceeds `expected_total_issued`).

| Value | Behaviour |
|-------|-----------|
| _(not set)_ | Defaults to **Medium** (`0.5`) on mismatch; **Info** (`0.1`) on match |
| Custom value | Applied on mismatch; **Info** is always used on match |

- **Info** alerts are always emitted when the supply is within bounds, regardless of this setting.
- This field only affects the severity of anomaly alerts.

---

## Alert Metadata

Every alert emitted by this monitor includes the following fields in its metadata:

| Field | Description |
|-------|-------------|
| `monitored_contract` | Address of the token contract being monitored |
| `expected_supply` | Configured `expected_total_supply` value |
| `expected_issued` | Configured `expected_total_issued` value |
| `current_issued` | Live total supply queried from Dune Analytics (adjusted for decimals) |
| `current_minted` | Total tokens minted to date (adjusted for decimals) |
| `current_burned` | Total tokens burned to date (adjusted for decimals) |

---

## Alert Types

| Event Type | Severity | Trigger Condition |
|---|---|---|
| `total_supply_issued_match` | Info | `current_issued ≤ expected_total_issued` |
| `total_supply_issued_mismatch` | Warning (or custom) | `current_issued > expected_total_issued` |

