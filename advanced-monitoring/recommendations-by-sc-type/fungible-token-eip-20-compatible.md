# Fungible Token (EIP-20 compatible)

Implementations may have some differences, but all requirements of the [standard](https://eips.ethereum.org/EIPS/eip-20) must be met.&#x20;

### Monitor the minting of new tokens

**Trigger:** Event Parameter _(coming soon)_

**Parameters:**

* **event:**  Transfer
* **argument:** from
* **comparator:** equal to
* **value:** 0x0000000000000000000000000000000000000000

**Severity:** Critical&#x20;

**Alerts:** On

**Use cases:**&#x20;

* If your contract involves a one-time minting of the total supply, then minting new tokens after deployment signals an issue in the contract that has been exploited.
