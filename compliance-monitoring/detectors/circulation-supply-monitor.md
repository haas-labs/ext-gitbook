---
description: >-
  Monitor token minting/burning events, track circulating supply changes, and detect unexpected inflation/deflation that could impact token economics.
---

# Circulation Supply Monitor

**Behavior**  

* Detects ERC20 Transfer events from/to zero addresses (mints/burns)
* Calculates supply changes and compares against thresholds
* Validates against expected inflation/deflation schedules
* Provides real-time alerts for significant supply modifications

**Use cases**  

* Token Launch Monitoring: Detect unauthorized minting during token launches to prevent supply manipulation and maintain fair distribution
* Staking Protocol Security: Monitor for unexpected token burning in staking contracts that could affect user rewards and protocol sustainability
* Governance Token Protection: Alert on unexpected supply changes that could dilute voting power or manipulate DAO governance decisions

