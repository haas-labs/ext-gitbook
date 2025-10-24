---
description: Monitors blocks for Block anomalies.Anomalies includes transaction count (low/large), large block size, large number of Events, etc
---

# Block Anomaly
 
**Behavior**  
Transactions Count - Very small or very large transaction count in a block  
Events Count - Very large or very small events count  
Block size - Very small or very large block size  
Gas - Large and small Gas usage in block

**Use cases**
* Network Health & Stability Monitoring -  A blockchain suddenly produces blocks with abnormally low transaction counts or gas usage, which can signal network congestion, validator downtime, or censorship attacks. The detector flags unusual block characteristics in real time, allowing infra teams (validators, node operators, custodians) to quickly investigate.

* Exploit & Attack Detection - Certain exploits (e.g., MEV spam attacks, denial-of-service attacks, reorg attempts) generate blocks with extremely high transaction/event counts or inflated block sizes. The detector catches these anomalies immediately, correlating them with attack patterns (e.g., repeated reverts, flash-loan spam, or flood of small transfers).

* Compliance & Forensics Auditing - Regulators, auditors, or compliance teams require visibility into abnormal block patterns that may indicate illicit activities (e.g., mixers, mass token distributions, laundering via event spam). The detector provides a structured log of block anomalies (e.g., spikes in event counts or gas usage), which can be cross-referenced with suspicious addresses and contracts.