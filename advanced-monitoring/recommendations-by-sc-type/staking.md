# Staking

A staking contract is a type of smart contract that enables users to hold and lock up a certain amount of cryptocurrency or tokens as collateral in order to participate in a blockchain network's consensus process and earn rewards.&#x20;

### Monitor withdrawal of tokens before the end of the staking period

**Trigger:** ERC20 Transfer more than n&#x20;

**Parameters:**

* **Token address:** the address of the collateral token
* **Amount:** should be as low as possible, for example less than the minimum deposit amount
* **Destination:** from

**Severity:** see the use cases&#x20;

**Alerts:** see the use cases

**Use cases:**&#x20;

* If a contract prohibits withdrawals before the end of the staking period, triggering this trigger may indicate a vulnerability in the contract. In this case, **we recommend setting the severity to critical and enabling alerts.**&#x20;
* If a contract allows for early withdrawals, then such a trigger would simply reflect that some users have chosen to withdraw tokens for some reason. In this case, **we recommend setting the severity to low.**

### Monitor unexpectedly large deposit amounts

**Trigger:** ERC20 Transfer more than n&#x20;

**Parameters:**

* **Token address:** the address of the collateral token
* **Amount:** should be 5-10% more than the largest expected deposit
* **Destination:** to

**Severity:** Medium&#x20;

**Alerts:** On

**Use cases:**&#x20;

* Unexpectedly large amount of deposit may be a sign that the attacker has found a vulnerability in the smart contract and is making a large deposit to get more profit from the exploit.

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
