---
description: >
  Tracks token minting and burning in real time to maintain a live circulating
  supply counter, alerting on significant supply changes and deviations from a
  user-defined inflation/deflation schedule.
---

# Circulation Supply Detector

## Overview

The Circulation Supply Detector processes every incoming transaction and inspects its logs for ERC-20 `Transfer` events that represent token minting or burning. It maintains an in-memory circulating supply counter and fires alerts in two scenarios:

1. **Supply change alert** — a single mint or burn moves the circulating supply by more than the configured percentage threshold
2. **Unexpected inflation/deflation alert** — the actual supply change on a scheduled date differs from the expected value defined in the inflation/deflation schedule


> ⚠️ Inflation/deflation schedule checks occur at most **once per calendar day** — on the first transaction processed after midnight UTC.

---

## Threshold

The minimum relative supply change (as a decimal fraction) that triggers a `circulation_supply_change` alert. Applied independently to each mint and burn event.

| Field | Type | Required |
|-------|------|----------|
| `threshold` | `float` | Yes |

| Example value | Meaning |
|---------------|---------|
| `0.01` | Alert when supply changes by ≥ 1% in a single event |
| `0.05` | Alert when supply changes by ≥ 5% in a single event |

The comparison uses the **absolute value** of the change, so both large mints and large burns trigger at the same threshold.

---

## Locker Addresses

A list of contract addresses that hold locked or vested tokens. Their balances are **excluded** from the circulating supply when the initial supply is fetched from the Transpose API at startup.

| Field | Type | Default |
|-------|------|---------|
| `locker_address` | `list[string]` | `[]` |

Common examples include team vesting contracts, treasury multisigs, and future-reserve allocations. Tokens held in these addresses are not considered to be in active circulation.

> Locker addresses affect only the **initial supply baseline**. Runtime mint/burn events are always applied to the full tracked supply regardless of recipient.

---

## Inflation/Deflation Schedule

An optional list of expected supply change events on specific calendar dates. When the schedule is configured the detector verifies, on the first transaction of each day, whether the previous day's actual supply change matched the expectation.

| Sub-field | Type | Description |
|-----------|------|-------------|
| `expected_date` | `string` (`YYYY-MM-DD`) | The date on which the scheduled supply change should occur |
| `expected_change` | `float` | Expected fractional change (positive = inflation, negative = deflation) |

**Examples:**

| Schedule entry | Meaning |
|---|---|
| `expected_date: 2025-06-01`, `expected_change: 0.05` | Expect a 5% increase in supply on 1 June 2025 |
| `expected_date: 2025-09-01`, `expected_change: -0.02` | Expect a 2% decrease in supply on 1 September 2025 |

If the actual supply change on a scheduled date does not match `expected_change`, an `inflation_detected` alert is fired.

> The schedule check compares the **end-of-previous-day supply** against the supply at the time of the first transaction of the next day. If no schedule entry exists for a date, no check is performed.

---

## Alert Types

| Event Type | Trigger |
|---|---|
| `circulation_supply_change` | A single mint or burn changes the supply by ≥ `threshold` |
| `inflation_detected` | The actual daily supply change differs from the scheduled `expected_change` |

---

## Alert Metadata

### `circulation_supply_change`

Fired when a mint or burn in a single transaction causes the supply to move beyond the threshold.

| Field | Description |
|-------|-------------|
| `tx_hash` | Hash of the transaction containing the mint or burn |
| `tx_from` | Sender of the outer transaction |
| `tx_to` | Recipient of the outer transaction |
| `contract` | Monitored contract address (the transaction target) |
| `monitored_contract` | Token contract address that emitted the Transfer event |
| `recipient` | Address that received the minted tokens, or the burn address |
| `prev_supply` | Circulating supply before the event (raw integer) |
| `curr_supply` | Circulating supply after the event (raw integer) |
| `diff` | Raw token amount minted (positive) or burned (negative) |
| `change_percentage` | Relative supply change, formatted to 4 decimal places (e.g. `1.2345%`) |

### `inflation_detected`

Fired once per day when the previous day's supply change deviates from the scheduled expectation.

| Field | Description |
|-------|-------------|
| `tx_hash` | Hash of the first transaction of the new day that triggered the check |
| `tx_from` | Sender of that transaction |
| `tx_to` | Recipient of that transaction |
| `contract` | Monitored contract address |
| `monitored_contract` | Token contract address |
| `previous_supply` | End-of-day supply from the previous day |
| `current_supply` | Supply at the time of the check |
| `current_change` | Actual fractional supply change, formatted to 10 decimal places |
| `expected_change` | Scheduled expected fractional change, formatted to 10 decimal places |
| `expected_date` | The date (`YYYY-MM-DD`) for which the deviation was detected |

---

## FAQ

**What is a locker address?**
A contract that holds locked or vested tokens excluded from the circulating supply — such as team allocation wallets, vesting contracts, or reserve funds. These are subtracted from the total supply when the baseline is established at startup.

**How should I define the threshold?**
Use a decimal fraction representing the minimum percentage change that warrants an alert. For example, `0.01` means 1%. The detector applies the threshold to the absolute size of each individual mint or burn relative to the supply at the time of the event.

**What is the inflation/deflation schedule?**
A predefined list of dates and expected fractional supply changes. The detector checks the previous day's actual change against the schedule each morning. Positive values indicate expected inflation; negative values indicate expected deflation.
