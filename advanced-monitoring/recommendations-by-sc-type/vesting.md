# Vesting

A vesting contract is a type of smart contract that is used to manage the release of tokens or other assets to a recipient over a specified period of time.

### Monitor any withdrawals from the contract when the cliff period has not ended

**Trigger:** ERC20 Transfer more than n&#x20;

**Parameters:**

* **Token address:** the address of the vested token
* **Amount:** should be as small as possible, e.g. 1 wei
* **Destination:** from

**Severity:** Critical&#x20;

**Alerts:** On

**Use cases:**&#x20;

* Any withdrawal of funds from the contract before the end of the cliff period clearly signals that there is an issue in the contract and it has been exploited. Remove this trigger after the cliff period is over.

### Monitor unexpected large withdrawals

**Trigger:** ERC20 Transfer more than n&#x20;

**Parameters:**

* **Token address:** the address of the vested token
* **Amount:** should be greater than the maximum possible amount by 1 wei
* **Destination:** from

**Severity:** Critical&#x20;

**Alerts:** On

**Use cases:**&#x20;

* Withdrawing an amount from a contract that exceeds the maximum possible under current vesting rules means that there is an issue with the contract and it has been exploited.

### Set up a list of all beneficiaries

**Trigger:** Whitelisted Callers

**Parameters:**

* **Whitelisted Addresses:** list of addresses of all beneficiaries

**Severity:** Critical&#x20;

**Alerts:** On

**Use cases:**&#x20;

* Usually, after the start of vesting, the list of those who interact with the contract is the list of its beneficiaries. If someone not from this list interacts with the contract it could be a sign that the contract has been compromised or hacked in some way.
