# Non-Fungible Token (EIP-721 compatible)

Implementations may have some differences, but all requirements of the [standard](https://eips.ethereum.org/EIPS/eip-721) must be met.&#x20;

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

* If your NFT collection is already complete and you are not expecting any new tokens to be created, then it is particularly important to monitor the creation of new tokens with unique IDs.
