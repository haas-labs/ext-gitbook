# Lending

Lending smart contracts are smart contracts that enable users to lend and borrow assets.

### Monitor unexpectedly large borrowed amounts

**Trigger:** ERC20 Transfer more than n&#x20;

**Parameters:**

* **Token address:** the address of the collateral token
* **Amount:** should be 5-10% more than the largest expected deposit
* **Destination:** from

**Severity:** Medium&#x20;

**Alerts:** On

**Use cases:**&#x20;

* Borrowing an unusually large amount may be a sign that the attacker has found a vulnerability in the contract and is trying to exploit it.
