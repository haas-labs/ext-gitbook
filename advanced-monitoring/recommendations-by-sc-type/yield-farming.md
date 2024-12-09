# Yield Farming

It is a type of smart contract that enables users to earn rewards by providing liquidity to decentralized finance (DeFi) protocols. These rewards may be paid in the form of new tokens, cryptocurrency, or other forms of value.

### Monitor unexpectedly large deposit amounts

**Trigger:** ERC20 Transfer more than n&#x20;

**Parameters:**

* **Token address:** the address of the collateral token
* **Amount:** should be 5-10% more than the largest expected deposit
* **Destination:** to

**Severity:** Medium&#x20;

**Alerts:** On

**Use cases:**&#x20;

* An unexpectedly large amount of deposit may be a sign that the attacker has found a vulnerability in the smart contract and is making a large deposit to get more profit from the exploit.

### Monitor withdrawal of an amount that is greater than the largest deposit

**Trigger:** ERC20 Transfer more than n&#x20;

**Parameters:**

* **Token address:** the address of the collateral token
* **Amount:** should be greater than the largest deposit by at least 1 wei
* **Destination:** from

**Severity:** Critical&#x20;

**Alerts:** On

**Use cases:**&#x20;

* If there is a withdrawal from the contract for an amount greater than the largest deposit, then this is a clear sign that the contract has been hacked.
