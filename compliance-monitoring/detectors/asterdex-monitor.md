---
description: >
  Monitors an Aster DEX account for portfolio value changes and market price
  movements, alerting when user-defined conditions are met.
---

# Aster DEX Monitor

## Overview

The Aster DEX Monitor tracks financial activity on the [Aster DEX](https://asterdex.com) exchange. It supports two independent monitoring modes:

- **Portfolio mode** — monitors the total USD value of an account's holdings, requiring API credentials
- **Market mode** — monitors the spot price of one or more assets using public endpoints, no credentials required

Each mode evaluates a **flexible condition expression** on every check. An alert is fired only when the condition evaluates to `true`.

> ℹ️ To monitor both portfolio value and market prices simultaneously, create **two separate detector instances** — one in each mode.
>
> ⚠️ All prices are sourced as `{ASSET}USDT` pairs from the Aster DEX public ticker endpoint. Assets without a USDT pair are skipped in portfolio valuation.

---

## Monitoring Mode

Selects which data source and condition type to evaluate.

| Value | Description | Auth required |
|-------|-------------|---------------|
| `portfolio` | Monitors total account balance value in USD | Yes (API key + secret) |
| `market` | Monitors individual asset spot prices | No |

---

## Portfolio Mode

### API Key / API Secret

Credentials used to authenticate against the Aster DEX private account endpoint (`/api/v1/account`). Required only when `monitoring_mode` is `portfolio`.

Requests are signed using **HMAC SHA256** with a timestamp, following the Binance-compatible API signature scheme.

| Field | Type | Required |
|-------|------|----------|
| `api_key` | `string` | Yes (portfolio mode) |
| `api_secret` | `string` | Yes (portfolio mode) |

### Portfolio Condition

A condition expression evaluated against the **total account portfolio value in USD**. An alert is sent when the condition is `true`.

| Field | Type | Default |
|-------|------|---------|
| `portfolio_condition` | `string` | `">> 5%"` |

See the [Condition Syntax](#condition-syntax) section for the full list of supported expressions.

**Examples:**

| Condition | Behaviour |
|-----------|-----------|
| `>> 5%` | Alert once when portfolio changes by more than 5% since last check |
| `> 10%` | Alert every check while portfolio has changed more than 10% |
| `< 10000000` | Alert when portfolio value drops below $10,000,000 |
| `>> ++1000000` | Alert once when portfolio increases by more than $1,000,000 |

### Mock Account Data _(testing only)_

Bypasses the `/api/v1/account` API call and uses a static JSON payload instead. Asset prices are still fetched from live public endpoints.

| Field | Type | Default |
|-------|------|---------|
| `mock_account_data` | `string` (JSON) | `null` |

**Format:**
```json
{"balances": [{"asset": "BTC", "free": "100", "locked": "0"}]}
```

---

## Market Mode

### Monitored Assets

A list of assets and per-asset conditions evaluated against the current **spot price** (in USDT).

| Sub-field | Type | Default | Description |
|-----------|------|---------|-------------|
| `asset` | `string` | — | Asset symbol, e.g. `BTC`, `ETH` |
| `condition` | `string` | `"> 10"` | Condition evaluated against the USDT price |

Each asset is checked independently. A separate alert is emitted for each asset whose condition is met.

**Examples:**

| Asset config | Behaviour |
|---|---|
| `asset: BTC`, `condition: >> 10` | Alert once when BTC price changes by more than $10 since last check |
| `asset: ETH`, `condition: > 5%` | Alert every check while ETH has moved more than 5% |
| `asset: BNB`, `condition: < -15%` | Alert when BNB price drops more than 15% |

---

## Check Interval

Controls how often each monitor configuration is evaluated, in seconds. Each configuration can have its own independent interval, enabling different monitors to run at different frequencies.

| Field | Type | Default | Range |
|-------|------|---------|-------|
| `check_interval` | `int` | `300` | `60` – `86400` |

- Set to `3600` for hourly comparisons (price change over the last hour)
- Set to `86400` for 24-hour comparisons (price change over the last day)
- Minimum of `60` seconds is enforced to avoid API rate limits

> On startup, each monitor initialises its comparison baseline from **24 hours ago** to avoid flooding alerts on first run.

---

## Condition Syntax

Both `portfolio_condition` and per-asset `condition` fields share the same expression syntax.

### Operators

| Operator | Meaning |
|----------|---------|
| `>` | Greater than (uses absolute value by default) |
| `<` | Less than (uses absolute value by default) |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |
| `=` | Equals (exact match) |
| `!=` | Not equals |
| `>>` | Greater than — **triggers once** (edge detection) |
| `<<` | Less than — **triggers once** (edge detection) |

### Value Modifiers

| Modifier | Meaning | Example |
|----------|---------|---------|
| _(none)_ | Absolute value comparison | `> 50000` — alert when value > 50,000 |
| `%` suffix | Percentage change since last check | `> 5%` — alert when change > 5% |
| `++` prefix | Positive delta (increase) | `> ++1000` — alert when increase > 1,000 |
| `--` prefix | Negative delta (decrease) | `> --500` — alert when decrease > 500 |
| `+` / `-` sign | Explicit sign; disables absolute value handling | `> -15` — alert when value is greater than −15 |

Modifiers can be combined: `>> ++25%` — alert **once** when the percentage increase exceeds 25%.

### Once Operators (`>>` / `<<`)

The `>>` and `<<` operators implement **edge detection** — the alert fires only on the transition from `false` to `true`. Once triggered, the condition must return to `false` before it can fire again.

Use `>` / `<` if you want an alert **every check** while the condition holds.

### Examples

| Expression | Behaviour |
|------------|-----------|
| `> 5%` | Alert every check while change > 5% (absolute) |
| `>> 5%` | Alert once when change > 5%, then wait for it to recover |
| `< -15%` | Alert when price drops more than 15% |
| `< 10000000` | Alert when value drops below 10,000,000 |
| `>> ++100000` | Alert once when value increases by more than 100,000 |
| `<< --50000` | Alert once when value decreases by more than 50,000 |

---

## Severity

Severity is **automatically calculated** based on the magnitude of the change. It is not a user-configurable field.

### Portfolio mode

| Change magnitude | Severity |
|-----------------|----------|
| ≥ 50% | Critical (`0.75`) |
| ≥ 25% | High (`0.5`) |
| ≥ 10% | Medium (`0.25`) |
| < 10% | Low (`0.15`) |

For absolute value conditions, severity is derived from the ratio of the current value to the threshold:

| Current / Threshold | Severity |
|--------------------|----------|
| < 25% | Critical (`0.75`) |
| < 50% | High (`0.5`) |
| < 75% | Medium (`0.25`) |
| ≥ 75% | Low (`0.15`) |

### Market mode

| Price change magnitude | Severity |
|-----------------------|----------|
| ≥ 50% | Critical (`0.75`) |
| ≥ 25% | High (`0.5`) |
| ≥ 10% | Medium (`0.25`) |
| < 10% | Low (`0.15`) |

If no previous price is available (first check), severity defaults to **Medium** (`0.25`).

---

## Alert Types

| Event Type | Mode | Trigger |
|---|---|---|
| `asterex.portfolio.condition_met` | Portfolio | Portfolio condition expression evaluates to `true` |
| `asterex.market.condition_met` | Market | Per-asset price condition evaluates to `true` |

---

## Alert Metadata

### `asterex.portfolio.condition_met`

| Field | Description |
|-------|-------------|
| `current_value` | Current portfolio value in USD (formatted, e.g. `$1,234,567.89`) |
| `condition` | The condition expression that was met |
| `previous_value` | Portfolio value at the previous check _(if available)_ |
| `change_amount` | Absolute USD change since last check _(if available)_ |
| `change_percent` | Percentage change since last check _(if available)_ |

### `asterex.market.condition_met`

| Field | Description |
|-------|-------------|
| `asset` | Asset symbol (e.g. `BTC`) |
| `symbol` | Trading pair (e.g. `BTCUSDT`) |
| `current_price` | Current asset price in USD |
| `previous_price` | Price at the previous check _(if available)_ |
| `price_change` | Percentage change since last check _(if available)_ |
| `price_change_amount` | Absolute USD change since last check _(if available)_ |
| `condition` | The condition expression that was met |
