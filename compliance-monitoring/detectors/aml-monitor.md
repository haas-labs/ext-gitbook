---
description: >-
  AML Detector monitors on-chain interaction with your Contract from addresses
  and alerts if address is in AML lists
---

# AML Monitor

### AML List

Extractor supports the following address lists:

* **Cybercrime**
* **Sanctions**
* **Suspecious**

Upon AML address detection, alert will be generated with user-assigned `severity` . Detailed information about AML address metadata will contain:

* `source` (which dataset or provider metadata was extracted from)
* `score`: address risk score from 0 to 100.0 (where 100.0 is the highest risk score)
* `type_name`: address origin name
* `owner`: address owner (from origin)

Alert example:

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

