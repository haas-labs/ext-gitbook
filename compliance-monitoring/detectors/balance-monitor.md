---
description: >
  Monitors the native coin and ERC-20 token balances of accounts or contracts
  in real time, alerting when any tracked balance falls at or below a configured
  minimum threshold.
---

# Balance Monitor

## Overview

The Balance Monitor watches the balance of one or more addresses on a blockchain network and fires an alert whenever a balance drops to or below a configured threshold. Both the **native coin** (e.g. ETH) and any number of **ERC-20 tokens** can be monitored per address, each with its own independent threshold.

Checks are triggered in two ways:

- **On startup** — the current balance of every configured address is fetched immediately at the latest block and compared against the threshold
- **On each relevant transaction** — a balance re-check is performed whenever the monitored address appears as the sender or recipient of a native transfer, or as the sender in an ERC-20 `Transfer` event

> ℹ️ An alert is suppressed if the balance has not changed since the previous check, preventing duplicate notifications for the same state.
>
> ⚠️ Alerts fire when `balance ≤ threshold` — equality also triggers.

---

## Native Token

Configuration for monitoring the native coin balance of the monitored address.

### Balance Threshold

The minimum acceptable balance, expressed as a **raw integer** (i.e. in the smallest unit, without decimal adjustment).

| Field | Type | Default |
|-------|------|---------|
| `balance_threshold` | `int` | `200000000000000000` |

The default corresponds to **0.2 ETH** (2 × 10¹⁷ wei). Adjust based on the token and expected balance range.

### Decimals

The number of decimal places used by the native coin. Applied only for human-readable display in the alert description — the threshold comparison is always performed in raw units.

| Field | Type | Default |
|-------|------|---------|
| `decimals` | `int` | `18` |

---

## ERC-20 Tokens

A list of ERC-20 token contracts to monitor for the same address. Each entry is checked independently.

| Sub-field | Type | Default | Description |
|-----------|------|---------|-------------|
| `address` | `string` | — | ERC-20 token contract address |
| `balance_threshold` | `int` | `300000000000` | Minimum acceptable token balance (raw units) |
| `decimals` | `int` | `8` | Token decimal precision (for display only) |
| `symbol` | `string` | `"UNSPECIFED"` | Token symbol; verified on-chain at startup |

**Symbol verification:** On initialisation the monitor calls `symbol()` on each ERC-20 contract and compares the result to the user-supplied value. If they differ, the on-chain symbol takes precedence and a warning is logged. If the on-chain call fails, the user-supplied symbol is used as a fallback.

Multiple ERC-20 tokens can be added to a single configuration. Each token fires its own independent alerts.

---

## Severity

A single severity level applied to all alerts — both native and ERC-20 — for this configuration.

| Field | Type | Default |
|-------|------|---------|
| `severity` | `float` | `0.15` (Low) |

---

## How Monitoring Works

### Native coin

A balance re-check is triggered for a monitored address when a transaction has that address as either **sender (`from`)** or **recipient (`to`)**. The native balance is then queried at the transaction's block number.

### ERC-20 tokens

A balance re-check is triggered when a `Transfer` event with a **non-zero value** is detected in the transaction logs and its `from` address matches a monitored address. The ERC-20 `balanceOf` is called at the transaction's block number.

In both cases, if the new balance equals the previously cached balance, no alert is emitted.

---

## Alert Types

| Event Type | Trigger |
|---|---|
| `balance_threshold` | Current balance ≤ configured threshold |

---

## Alert Metadata

| Field | Description |
|-------|-------------|
| `tx_hash` | Hash of the triggering transaction (empty string if triggered on startup) |
| `tx_from` | Sender of the triggering transaction (empty string if triggered on startup) |
| `tx_to` | Recipient of the triggering transaction (empty string if triggered on startup) |
| `token` | Token contract address (`address` for ERC-20; monitored address for native) |
| `symbol` | Token symbol resolved on-chain, or `null` for native coin |
| `value` | Transaction value, or current balance if triggered on startup |
| `monitored_contract` | The monitored address whose balance is below threshold |
| `balance` | Current raw balance (not adjusted for decimals) |
| `threshold` | Configured threshold (raw units) |
| `desc` | Human-readable summary: `Balance <address> below threshold (<balance>, <threshold>)` |

---

## FAQ

**What asset types can be monitored?**
Native coin (e.g. ETH, BNB, MATIC) and any number of ERC-20 tokens — all within a single detector configuration.

**Are thresholds in USD?**
No. Thresholds are expressed in raw on-chain units. For 18-decimal tokens, multiply the desired amount by 10¹⁸.

**When is the balance checked?**
Immediately on startup (at the latest block) and again on every transaction that involves the monitored address — either as a native transfer participant or as a sender in an ERC-20 Transfer event.

**What happens if the ERC-20 `symbol()` call fails?**
The user-supplied symbol is used as a fallback. The error is logged but does not prevent monitoring.

**Will I receive repeated alerts while the balance stays below the threshold?**
Yes — an alert is emitted on every transaction that causes a balance change while the balance remains at or below the threshold. If the balance does not change between transactions, no duplicate alert is sent.
