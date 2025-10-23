---
description: Monitors DNS for potential attacks (like DNS Hijacking)
---

# DNS Monitor

**Behavior**  
Monitors WHOIS and DNS server records
Start alerting about Domain expiration before Domain expiration date.

**Use cases**
* Domain Hijacking & Phishing Prevention - An attacker compromises DNS records and redirects users to a malicious clone of a DeFi protocol or exchange website. The detector continuously monitors DNS and WHOIS records for unauthorized changes (e.g., altered A, CNAME, or NS records).

* Operational Continuity & Domain Expiration Alerts - A project’s critical domain (API endpoint, wallet interface, or dApp front-end) is at risk of expiring because renewal was overlooked. The detector issues early warnings well before the expiration date.
What is monitored

<!-- * Changes to **Nameservers** records&#x20;
* Changes in resolved **IP** addresses
* Domain expiration -->



