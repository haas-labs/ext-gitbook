---
description: >
  Monitors ERC-20 Transfer events on a blockchain in real time and alerts when a
  transfer involving a monitored address exceeds a configured token amount threshold.
---

# Transfer Monitor

## Overview

The Transfer Monitor processes every incoming transaction in real time, inspects its event logs for ERC-20 `Transfer` events, and fires an alert whenever a transfer involving a **monitored address** exceeds the configured **threshold**.

A monitored address is matched against three roles in each Transfer event:

- **Token contract** — the address that emitted the Transfer event
- **Sender** — the `from` field of the Transfer
- **Recipient** — the `to` field of the Transfer

An alert is emitted if the raw transfer `value` is **greater than or equal to** the configured threshold.

---

## Threshold

The minimum transfer amount that triggers an alert. Specified as a **raw integer** — i.e. the on-chain value without decimal adjustment.

| Field | Type | Required |
|-------|------|----------|
| `threshold` | `float` | Yes |

For example, to alert on transfers of 1,000 USDC (6 decimals), set the threshold to `1000000000` (1000 × 10⁶).

> **How to define a threshold:** Specify the transfer amount that will trigger an alert in integer format, for example `1000000`.

---

## Monitored Address Matching

The detector checks each Transfer event against every configured monitored address. A match is registered if the monitored address appears in **any** of the following positions:

| Position | Description |
|----------|-------------|
| Token contract | The address that emitted the `Transfer` log |
| From | The sender of the transfer |
| To | The recipient of the transfer |

A single transaction may produce multiple alerts if it contains multiple matching Transfer events or if the same address appears in several configurations.

---

## Severity

Controls the severity level of the alert when the threshold is exceeded.

| Value | Behaviour |
|-------|-----------|
| _(not set)_ | Defaults to **Low** (`0.3`) |
| Custom value | Applied directly to the alert |

---

## Alert Metadata

Every alert includes the following metadata fields:

| Field | Description |
|-------|-------------|
| `tx_hash` | Hash of the transaction containing the Transfer |
| `tx_from` | Sender address of the outer transaction |
| `tx_to` | Recipient address of the outer transaction |
| `monitored_contract` | The configured monitored address that triggered the match |
| `sender` | The `from` address in the Transfer event |
| `recipient` | The `to` address in the Transfer event |
| `value` | Raw transfer amount (on-chain integer, not adjusted for decimals) |
| `token` | Address of the ERC-20 token contract |
| `symbol` | Token symbol resolved on-chain (falls back to token address if unavailable) |
| `threshold` | The configured threshold value |

---

## Alert Types

| Event Type | Trigger |
|---|---|
| `transfer_amount_threshold_exceed` | Transfer `value ≥ threshold` for a monitored address |
