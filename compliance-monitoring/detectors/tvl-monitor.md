---
description: >-
  Monitor Total Value Locked drops in native and ERC20 tokens with configurable thresholds to detect potential exploits or significant value movements.
---

# TVL Monitor

**Behavior**  

* Tracks balance changes between blocks for monitored addresses
* Supports both percentage and absolute thresholds
* Monitors native tokens and ERC20 tokens separately
* Provides real-time alerts for significant TVL drops

**Use cases**  

* DeFi Protocol Security: Detect potential exploits or large withdrawals that could indicate security issues or market manipulation
* Liquidity Pool Monitoring: Alert on significant liquidity removals that could impact trading and cause slippage
* Risk Management: Monitor institutional or whale movements that could signal market sentiment changes


**Detector Configuration**  
1. *Name* - Enter a descriptive name for your monitor, for example: "TVL Monitor".
2. *Token*
3. *Native Token*
4. *Native Token Threshold
<figure><img src="../../.gitbook/assets/tvl_faq.png" alt=""><figcaption></figcaption></figure>

**Alert example**
<figure><img src="../../.gitbook/assets/tvl_alert.png" alt=""><figcaption></figcaption></figure>