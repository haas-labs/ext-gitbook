---
description: >-
  Monitors Price changes of the Contract. Contract must be a Token contract (ERC-20 like)
---

# Price Monitor

**Behavior**  

When to query price:  
```cron``` - On Cron interval  
```block``` - On every new block

*Source*  
Where to query price:  
```oracle``` - On-chain Feed (Chainlink Oracle format)  
```coingecko``` - Coingecko Feed

*Currency*  
Currency of the price. Default is ```usd```.

*NOTE*: Currently supported only for ```coingecko``` feed.  
```oracle``` feed requires adding Chainlink Oracle Contracts in configuration

**Use cases**  

* Alert on sudden price drops of stablecoins or RWAs.
* Detect oracle price manipulation attempts in DeFi protocols.
* Track token price feeds for liquidation risk management.

