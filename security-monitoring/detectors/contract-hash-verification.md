---
description: >
  Monitors proxy contracts for upgrade events and verifies that the new
  implementation's bytecode checksum matches the expected value, alerting on
  any mismatch.
---

# Contract Hash Verification

## Overview

The Contract Hash Verification detector watches for `Upgraded` events emitted by proxy contracts on the EVM network. When an upgrade is detected, the detector fetches the bytecode of the new implementation contract via RPC, computes its hash, and compares it against a pre-configured expected checksum.

An alert is fired if the checksums do not match, indicating that an unexpected or unauthorised implementation was deployed.

There are two moments when verification runs:

- **On startup** — if `initial_check` is enabled, the current implementation's bytecode is verified immediately against the expected checksum before any transactions are processed
- **On upgrade** — every time an `Upgraded` event is detected in a transaction for a monitored proxy address

> ✅ No alert is sent when the checksum matches — a silent pass indicates successful verification.

---

## Contract Bytecode Checksum

The expected hash of the implementation contract's compiled bytecode. This is the value the detector compares against the live on-chain bytecode hash after every upgrade.

| Field | Type | Required |
|-------|------|----------|
| `checksum` | `string` | Yes |

The checksum must be computed using the same hash function selected in `hash_type`. To obtain it, hash the deployed bytecode of the known-good implementation contract.

---

## Hash Type

| Value | Algorithm |
|-------|-----------|
| `KECCAK` | Keccak-256 (standard EVM hash) |
| `SHA256` | SHA-256 |

---

## Initial Check

Controls whether the detector verifies the current implementation bytecode immediately on startup, before any upgrade event is observed.

| Field | Type | Default |
|-------|------|---------|
| `initial_check` | `bool` | `true` |

When enabled, the detector fetches and hashes the bytecode at the monitored proxy address during initialisation. If the hash does not match `checksum`, a `contract_checksum_mismatch` alert is emitted immediately with no associated transaction.

Use this to catch pre-existing mismatches or to validate the initial configuration before live monitoring begins.

---

## Alert Types

| Event Type | Trigger |
|---|---|
| `contract_checksum_mismatch` | Deployed bytecode hash does not match `checksum`, either on startup (if `initial_check: true`) or after an `Upgraded` event |

---

## Alert Metadata

| Field | Description |
|-------|-------------|
| `tx_hash` | Hash of the transaction that contained the `Upgraded` event (empty string if triggered on startup) |
| `tx_from` | Sender of that transaction (empty string if triggered on startup) |
| `tx_to` | Recipient of that transaction (empty string if triggered on startup) |
| `monitored_contract` | Address of the monitored proxy contract |
| `hash_type` | Hash algorithm used for verification (`KECCAK` or `SHA256`) |
| `deployed_hash` | Checksum computed from the live on-chain bytecode of the new implementation |
| `expected_hash` | The configured expected checksum |

---

## FAQ

**What triggers a verification check?**
Two things: an `Upgraded` event emitted by the monitored proxy contract in any transaction, and — if `initial_check` is enabled — the startup of the detector itself.

**What is the `Upgraded` event?**
A standard event emitted by OpenZeppelin-compatible upgradeable proxy contracts when their implementation address is changed. It carries the address of the new implementation as a field.

**What hash should I put in the `checksum` field?**
Compute the Keccak-256 (or SHA-256, if using that mode) of the **deployed bytecode** of the authorised implementation contract. This can be done with standard EVM tooling (e.g. `cast keccak $(cast code <address>)`).

**What happens if the checksums match?**
Nothing — no alert is sent. A match means the deployed implementation is the expected one.

**What happens if the checksum is not configured?**
The configuration is skipped entirely and a warning is logged. The address will not be monitored until a valid checksum is provided.
