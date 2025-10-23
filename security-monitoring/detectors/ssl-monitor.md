---
description: >-
  Monitors SSL certificate
---

# SSL Monitor

**Behavior**  
Detectors downloads SSL certificate and checks it for validity and trust.  

*Vadlidity:*

* CA chain is valid
* SSL is not expired
* Signature is correct

*Trust:*
* System CA trusts certificate (self-signed certificates are not trusted by default)

**Use cases**  
* Certificate Expiry Prevention: ensure the main website and dApp endpoints always have valid SSL/TLS certificates. The monitor regularly downloads the certificate and checks the expiration date.

* Trust and MITM Protection: verify that API’s SSL certificate remains trusted. If the certificate were to be replaced by an untrusted one due to misconfiguration or a man-in-the-middle attack, the monitor would flag the issue. The security team would get an immediate alert that the certificate chain or signature is invalid, allowing them to respond before users’ connections are affected.

* Multi-Service Certificate Management: An infrastructure team adds the SSL Monitor for all customer-facing services (websites, APIs, and mobile app backends). The monitor checks each service’s certificate chain for validity and ensures it’s signed by a known Certificate Authority. Should any service inadvertently serve an incorrect certificate (for instance, a staging certificate on production or a compromised certificate), the monitor will catch it. The team can then quickly replace the certificate or investigate, maintaining user trust and secure communications.