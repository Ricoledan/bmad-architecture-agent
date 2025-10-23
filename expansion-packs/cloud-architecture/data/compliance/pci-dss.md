# PCI-DSS Compliance (Payment Card Industry Data Security Standard)

## Overview

PCI-DSS is a set of security standards designed to ensure that all companies that accept, process, store, or transmit credit card information maintain a secure environment.

**Issuing Authority**: PCI Security Standards Council (Founded by Visa, MasterCard, American Express, Discover, JCB)
**Current Version**: PCI-DSS v4.0 (March 2022, fully effective March 2025)
**Applicability**: Any organization handling credit/debit card data
**Certification**: Annual assessment required

---

## Cardholder Data (CHD)

**Primary Account Number (PAN)**: The 13-19 digit credit card number
- Must be protected wherever stored, transmitted, or processed
- Can be truncated (only show last 4 digits)
- Cannot be stored after authorization (unless encrypted)

**Sensitive Authentication Data (SAD)**:
- Full magnetic stripe data
- CAV2/CVC2/CVV2/CID (3-4 digit security code)
- PINs/PIN blocks

**Must NEVER be stored after authorization, even if encrypted**

---

## Merchant Levels

| Level | Transaction Volume (Annual) | Validation Requirements |
|-------|----------------------------|------------------------|
| 1 | 6M+ transactions | Annual onsite assessment by QSA |
| 2 | 1M - 6M transactions | Annual Self-Assessment Questionnaire (SAQ) |
| 3 | 20K - 1M e-commerce transactions | Annual SAQ |
| 4 | <20K e-commerce, <1M other | Annual SAQ |

**QSA**: Qualified Security Assessor (third-party auditor)
**SAQ**: Self-Assessment Questionnaire (various types based on environment)

---

## 12 PCI-DSS Requirements (6 Goals)

### Build and Maintain a Secure Network and Systems

**1. Install and maintain network security controls**
- Firewalls and routers configured to restrict traffic
- Deny-by-default rule sets
- DMZ for public-facing systems
- No direct internet connection for cardholder data environment (CDE)

**2. Apply secure configurations to all system components**
- Change vendor-supplied defaults (passwords, keys)
- Disable unnecessary services and protocols
- Configuration standards for all components
- Quarterly vulnerability scans

### Protect Account Data

**3. Protect stored account data**
- Minimize data retention (don't store if not needed)
- Truncate PAN when full number not needed (show last 4)
- Render PAN unreadable: Encryption (AES-256), tokenization, hashing
- Never store SAD after authorization

**4. Protect cardholder data with strong cryptography during transmission**
- TLS 1.2+ for transmission over open/public networks
- Never send PAN via unencrypted email, IM, SMS
- Trusted certificates and keys

### Maintain a Vulnerability Management Program

**5. Protect all systems and networks from malicious software**
- Anti-malware on all systems (especially those commonly affected)
- Keep anti-malware up to date
- Automatic updates and scans
- Generate logs and review regularly

**6. Develop and maintain secure systems and software**
- Secure development practices (OWASP guidelines)
- Patch critical vulnerabilities within 30 days
- Regular vulnerability scans (quarterly)
- Penetration testing (annually and after significant changes)

### Implement Strong Access Control Measures

**7. Restrict access to system components and cardholder data by business need to know**
- RBAC - Grant access based on job function
- Least privilege principle
- Default deny for access to CDE

**8. Identify users and authenticate access to system components**
- Unique user IDs (no shared accounts)
- MFA for all non-console access to CDE
- MFA for all remote access (VPN, admin)
- Strong password policies (12+ characters, complexity)
- Account lockout after failed attempts

**9. Restrict physical access to cardholder data**
- Physical access controls (badges, biometrics)
- Distinguish between onsite personnel and visitors
- Visitor logs and escort requirements
- Secure all media (backup tapes, drives)

### Regularly Monitor and Test Networks

**10. Log and monitor all access to system components and cardholder data**
- Audit logs for all access to CHD
- Log review daily (automated tools)
- Retain logs for 1 year minimum (3 months readily available)
- Time synchronization (NTP)
- Protect logs from alteration

**11. Test security of systems and networks regularly**
- Quarterly vulnerability scans by ASV (Approved Scanning Vendor)
- Annual penetration testing
- Intrusion detection/prevention systems (IDS/IPS)
- File integrity monitoring (FIM) on critical files
- Network and host-based IDS

### Maintain an Information Security Policy

**12. Support information security with organizational policies and programs**
- Information security policy established and published
- Risk assessment performed at least annually
- Security awareness training for all personnel
- Incident response plan tested annually
- Service provider management (ensure PCI compliance)

---

## Cardholder Data Environment (CDE)

**CDE includes**:
- Systems that store, process, or transmit CHD
- Systems that directly connect to or support CDE
- Network segmentation to isolate CDE

**Network Segmentation Benefits**:
- Reduces scope of PCI assessment
- Isolates CDE from other systems
- Contains breaches

---

## Key Technical Controls

### Encryption
- **At Rest**: AES-256 for stored CHD
- **In Transit**: TLS 1.2+ (TLS 1.3 recommended)
- **Key Management**: Secure key generation, distribution, storage, rotation

### Tokenization
- Replace PAN with non-sensitive token
- Token has no mathematical relationship to PAN
- Reduces PCI scope significantly

### Masking/Truncation
- Display only last 4 digits of PAN
- First 6 and last 4 can be displayed together for business need

### Access Controls
- RBAC with least privilege
- MFA for all admin and remote access
- Unique user IDs
- Automatic logoff (15 minutes inactivity)

### Monitoring
- SIEM for log aggregation and analysis
- File integrity monitoring (FIM)
- IDS/IPS on network boundaries
- Daily log review

---

## PCI-DSS v4.0 Major Changes (Effective March 2025)

### New Requirements
- **MFA for all access to CDE** (not just remote)
- **Targeted risk analysis** for certain requirements
- **Account lockout after 10 failed attempts**
- **Phishing-resistant MFA** encouraged (FIDO2, WebAuthn)
- **Enhanced logging** (commands executed, audit log reviews)
- **Automated log review** required

### Customized Approach
- Allows organizations to meet security objectives using custom controls
- Must document how controls meet objectives
- Requires risk analysis and validation

---

## Penalties for Non-Compliance

### Financial Penalties
- $5,000 - $100,000 per month (imposed by card brands)
- Increased transaction fees
- Fines for data breaches ($50 - $90 per compromised card record)

### Operational Impact
- Loss of ability to process card payments
- Mandatory forensic audits
- Brand damage and customer loss
- Legal liability and lawsuits

---

## PCI-DSS Compliance Checklist

**Network Security**:
- [ ] Firewall configured with deny-by-default
- [ ] DMZ for internet-facing systems
- [ ] Network segmentation isolating CDE

**Data Protection**:
- [ ] CHD encrypted at rest (AES-256)
- [ ] CHD encrypted in transit (TLS 1.2+)
- [ ] SAD never stored after authorization
- [ ] PAN truncated when full number not needed

**Access Control**:
- [ ] RBAC implemented
- [ ] MFA for all access to CDE
- [ ] Unique user IDs (no shared accounts)
- [ ] Quarterly access reviews

**Monitoring**:
- [ ] Audit logs enabled for all CHD access
- [ ] Daily log review (automated)
- [ ] File integrity monitoring on critical systems
- [ ] IDS/IPS deployed

**Vulnerability Management**:
- [ ] Quarterly vulnerability scans by ASV
- [ ] Annual penetration testing
- [ ] Anti-malware deployed and updated
- [ ] Patch management (30-day SLA for critical)

**Testing**:
- [ ] Annual PCI assessment (SAQ or QSA)
- [ ] Quarterly network scans
- [ ] Incident response plan tested
- [ ] Security awareness training completed

---

## Resources

- [PCI Security Standards Council](https://www.pcisecuritystandards.org/)
- [PCI-DSS v4.0 Full Standard](https://www.pcisecuritystandards.org/document_library/)
- [Self-Assessment Questionnaires (SAQs)](https://www.pcisecuritystandards.org/document_library/)
- [Approved Scanning Vendors (ASV)](https://www.pcisecuritystandards.org/assessors_and_solutions/approved_scanning_vendors)
- [Qualified Security Assessors (QSA)](https://www.pcisecuritystandards.org/assessors_and_solutions/qualified_security_assessors)
