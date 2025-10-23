---
description: Monitors Gas price
---

# Gas Monitor

**Behavior**  
*Price Avg* Average Price threshold in gwei. (Ex: >150.0)
*Price Max* Max Price threshold in gwei. (Ex: >250.0)  
*Price Min* Min Price threshold in gwei. (Ex: <1.0)

**Use cases**
* Detect suspicious drops or high spike in gas usage that could indicate network attack.

* Trading Bot Fee Optimization: optimize transaction timing. The bot’s operators define a maximum gas price (say 150 gwei); if the current gas exceeds this, the Gas Monitor event tells the bot to hold off on executing trades.

* Network Congestion Safeguard: automatically safeguard users during extreme network congestion.
<!-- * Changes to **Nameservers** records&#x20;
* Changes in resolved **IP** addresses
* Domain expiration -->



