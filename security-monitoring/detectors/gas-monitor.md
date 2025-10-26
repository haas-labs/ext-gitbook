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


**Detector Configuration**  
1. *Name* - Enter a descriptive name for your monitor, for example: "Gas Monitor".
2. *Price Avg (gwei)*
3. *Price Max (gwei)*
4. *Price Min (gwei)*
5. *Cron*
<figure><img src="../../.gitbook/assets/gas_monitor_faq.png" alt=""><figcaption></figcaption></figure>

**Alert example**
<figure><img src="../../.gitbook/assets/gas_monitor_alert.png" alt=""><figcaption></figcaption></figure>



