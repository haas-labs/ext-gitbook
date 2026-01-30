---
title: Chainabuse Montior
---

**Behavior**\
Analyse and filter by LLM the malicious crypto activity to identify connection and pattern of latest reported threads with clients infrastructure as soon as possible to minimise harmful outcomes.

**Supported Categories:**

* PHISHING
* OTHER\_BLACKMAIL
* SEXTORTION
* RANSOMWARE
* IMPERSONATION
* FAKE\_RETURNS
* OTHER\_HACK
* FAKE\_PROJECT
* AIRDROP
* ROMANCE
* PIGBUTCHERING
* CONTRACT\_EXPLOIT
* DONATION\_SCAM
* SIM\_SWAP
* OTHER\_INVESTMENT\_SCAM
* JOB\_SCAM
* UPGRADE\_SCAM
* MAN\_IN\_THE\_MIDDLE\_ATTACK
* OTHER

**Use cases**

* Scam Alerts: get instant alerts when new scam reports (e.g. phishing or fake airdrops) appear on Chainabuse that target its brand or users. This allows security team to quickly warn customers and block any associated malicious addresses.
* Fraud Detection: scan incoming transactions against reported scam addresses. If a user’s wallet is involved with an address flagged for scams (like a romance or “pig butchering” scam), the system notifies the provider to investigate and potentially freeze the suspicious activity.
* Infrastructure Protection: A blockchain project team monitors Chainabuse reports for any mention of their smart contracts or tokens. For example, if a fake project or impersonation scam uses the project’s name, the Chainabuse Monitor will alert the team so they can address the issue.

**Detector Configuration**

1. _Name_ - Enter a descriptive name for your monitor, for example: "Chainabuse Monitor".
2. _Category(optional)_ - If empty, all categories will be alerted.
3. _Cron_ - Enter a cron expression to define the schedule.\
   Cron expression in Quartz syntax, milliseconds value or seconds/minutes/hours/days interval (e.g. `24 hours`)

<figure><img src="../../.gitbook/assets/chainabuse_monitor_faq.png" alt=""><figcaption></figcaption></figure>

**Alert example**

<figure><img src="../../.gitbook/assets/chainabuse_monitor_alert.png" alt=""><figcaption></figcaption></figure>
