---
description: >-
  Verify that upgraded proxy contract implementations match expected bytecode hashes, preventing malicious upgrades and ensuring code integrity. 
---

# Contract Hash Verification

**Behavior**  
* Listens for UPGRADED events from proxy contracts
* Fetches implementation bytecode and computes hash
* Compares against configured expected hashes
* Supports initial checks and real-time monitoring

**Use cases**  
* Proxy Security: Ensure only authorized contract upgrades are deployed, preventing malicious code injection in upgradeable contracts

* Multi-sig Governance: Verify that DAO-approved contract upgrades match the intended implementation before execution

* Audit Trail: Maintain cryptographic proof of contract versions for compliance and security audits