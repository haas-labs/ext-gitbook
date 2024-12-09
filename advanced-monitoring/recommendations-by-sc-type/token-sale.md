# Token Sale

Token Sale is a type of smart contract that is used to facilitate the sale of tokens to investors.

### Monitor withdrawal of funds from the contract before the sale is completed

**Trigger:** ERC20 Transfer more than n&#x20;

**Parameters:**

* **Token address:** the address of the selling token
* **Amount:** should be as small as possible, e.g. 1 wei
* **Destination:** from

**Severity:** Critical&#x20;

**Alerts:** On

**Use cases:**&#x20;

* In most cases, tokens can only be withdrawn after a successful sale. Remove this trigger after the sale is over.

### Monitor withdrawal of tokens for an amount greater than the sale limit

**Trigger:** ERC20 Transfer more than n&#x20;

**Parameters:**

* **Token address:** the address of the selling token
* **Amount:** should be greater than the sale limit by 1 wei
* **Destination:** from

**Severity:** Critical&#x20;

**Alerts:** On

**Use cases:**&#x20;

* If a limit is set for the sale of tokens per account, exceeding this limit signals an issue in the contract.
