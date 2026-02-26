---
description: >
  Tracks liquidity changes for ERC-20 tokens and native coins held by a monitored
  contract, alerting when the balance drops by more than a configured threshold
  between consecutive blocks.
---

# TVL Monitor

## Overview

The TVL Monitor watches the balance of specified assets held by a monitored contract and fires an alert whenever the balance drops by more than a configured threshold between the **previous block** and the **current block**.

Two asset types are supported independently:

- **Native coin** (ETH, MATIC, etc.) — monitored via direct RPC balance queries
- **ERC-20 tokens** — monitored via `balanceOf()` calls against each configured token contract

Both the native coin and each ERC-20 token have their own threshold, so drops in different assets trigger separate alerts.

> ⚠️ Thresholds apply to balance **drops** only. An increase in balance never triggers an alert.
> ⚠️ Alerts are only issued per-transaction — if the balance of a monitored asset did not change in a given transaction, no check is performed for that asset in that block.

---

## Monitoring Tokens

A list of ERC-20 token addresses to watch, each with its own drop threshold.

| Sub-field | Type | Default | Description |
|-----------|------|---------|-------------|
| `token` | `string` | — | ERC-20 token contract address |
| `threshold` | `string` | `"1%"` | Drop amount that triggers an alert (absolute or percentage) |

Multiple tokens can be added. Each token is checked independently — a drop in one does not affect others.

> **How to specify ERC-20 tokens:** provide the token contract address and a threshold for the value locked in that particular asset.

---

## Native Token

Enables monitoring of the network's native coin (e.g. ETH) balance held by the monitored contract.

| Field | Type | Default |
|-------|------|---------|
| `native_token_status` | `bool` | `true` |
| `native_token_threshold` | `string` | `"1%"` |

- Set `native_token_status` to `true` to enable native coin monitoring.
- `native_token_threshold` follows the same format as ERC-20 thresholds (absolute or percentage).

> Native coin monitoring is only triggered when a transaction carries a non-zero `value` directly to the monitored contract address.

---

## Threshold Format

Thresholds can be expressed in two formats:

| Format | Example | Behaviour |
|--------|---------|-----------|
| Percentage | `"5%"` | Alert when balance drops by ≥ 5% relative to the previous block |
| Absolute | `"1000000000000000000"` | Alert when balance drops by ≥ the raw integer amount (not adjusted for decimals) |

To use a percentage, include the `%` sign. To use an absolute value, enter a plain number string.

> **Note:** Thresholds are per-asset and are not denominated in USD. A `5%` threshold on USDC and a `5%` threshold on ETH are evaluated independently in their respective token units.

---

## Severity

A single severity level applied to all `balance_drop` alerts for this configuration.

| Field | Type | Default |
|-------|------|---------|
| `severity` | `float` | `0.5` (Medium) |

Internal errors (e.g. RPC failures during balance resolution) always emit at a fixed severity of `0.7` (High), regardless of this setting.

---

## Alert Types

| Event Type | Trigger | Severity |
|---|---|---|
| `balance_drop` | Balance of a monitored asset dropped by ≥ threshold between blocks | Configured (default `0.5`) |
| `tvl_monitor_error` | RPC error during `balanceOf` call for a tracked token | Fixed `0.7` |

---

## Alert Metadata

### `balance_drop`

| Field | Description |
|-------|-------------|
| `tx_hash` | Hash of the transaction that caused the balance change |
| `tx_from` | Sender of the outer transaction |
| `tx_to` | Recipient of the outer transaction |
| `monitored_contract` | Address of the monitored contract |
| `token` | Token address (`0x000…0000` for native coin) |
| `curr_balance` | Balance at the current block (raw on-chain value) |
| `prev_balance` | Balance at the previous block (raw on-chain value) |
| `drop_percentage` | Computed drop — percentage or absolute value depending on threshold type |

### `tvl_monitor_error`

| Field | Description |
|-------|-------------|
| `tx_hash` | Hash of the transaction being processed when the error occurred |
| `tx_from` | Sender of the outer transaction |
| `tx_to` | Recipient of the outer transaction |
| `monitored_contract` | Address of the monitored contract |
| `token` | Token address that caused the RPC failure |
| `desc` | Error message describing the failure |

---

## FAQ

**Which assets can be tracked?**
ERC-20 tokens and the native coin of the network (e.g. ETH on Ethereum).

**Will I be notified about assets I haven't configured?**
No. Only balances of explicitly configured tokens and the native coin (when enabled) are checked.

**Is the threshold in USD?**
No. Each threshold is evaluated against the raw balance of the specific asset, not its USD equivalent.
