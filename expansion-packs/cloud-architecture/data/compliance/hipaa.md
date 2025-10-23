# HIPAA Compliance (Health Insurance Portability and Accountability Act)

## Overview

HIPAA establishes national standards for protecting sensitive patient health information. Compliance is mandatory for healthcare providers, health plans, healthcare clearinghouses, and their business associates.

**Issuing Authority**: U.S. Department of Health and Human Services (HHS)
**Effective Date**: 1996 (Privacy Rule: 2003, Security Rule: 2005, HITECH Act: 2009)
**Applicability**: Covered entities and business associates handling PHI

---

## Protected Health Information (PHI)

PHI includes any information that can identify a patient and relates to:
- Past, present, or future physical/mental health
- Healthcare services provided
- Payment for healthcare services

**18 HIPAA Identifiers** (must be removed for de-identification):
1. Names
2. Geographic subdivisions smaller than state
3. Dates (birth, admission, discharge, death)
4. Telephone numbers
5. Fax numbers
6. Email addresses
7. Social Security numbers
8. Medical record numbers
9. Health plan beneficiary numbers
10. Account numbers
11. Certificate/license numbers
12. Vehicle identifiers
13. Device identifiers/serial numbers
14. Web URLs
15. IP addresses
16. Biometric identifiers
17. Full-face photos
18. Any other unique identifying number/characteristic

---

## HIPAA Rules

### Privacy Rule
- Patient rights to access and control PHI
- Minimum necessary standard (only access needed PHI)
- Notice of Privacy Practices required
- Authorization required for most disclosures
- Patient rights: access, amendment, accounting

### Security Rule (3 Safeguards)

**Administrative Safeguards** (9 standards):
1. Security Management Process (risk analysis, risk management)
2. Assigned Security Responsibility (CISO/Security Officer)
3. Workforce Security (authorization, clearance, termination)
4. Information Access Management (role-based access)
5. Security Awareness and Training
6. Security Incident Procedures
7. Contingency Plan (backup, disaster recovery, emergency mode)
8. Evaluation (annual security assessments)
9. Business Associate Contracts

**Physical Safeguards** (4 standards):
1. Facility Access Controls (badge systems, visitor logs)
2. Workstation Use (secure workstations, screen privacy)
3. Workstation Security (physical locks, cable locks)
4. Device and Media Controls (disposal, media reuse, accountability)

**Technical Safeguards** (5 standards):
1. Access Control (unique user IDs, automatic logoff, encryption)
2. Audit Controls (audit logs, log review)
3. Integrity (data integrity validation, digital signatures)
4. Person or Entity Authentication (MFA, strong passwords)
5. Transmission Security (encryption in transit, TLS)

### Breach Notification Rule
- Notify HHS within 60 days of breach (500+ individuals: immediate)
- Notify affected individuals within 60 days
- Notify media if breach affects 500+ individuals in same state
- Maintain breach log for smaller breaches (annual reporting)

### Enforcement Rule
- Civil penalties: $100 - $50,000 per violation (up to $1.5M per year)
- Criminal penalties: $50,000 - $250,000 and 1-10 years imprisonment

---

## Key Technical Requirements

### Encryption
- **At Rest**: AES-256 (not required but recommended as "safe harbor")
- **In Transit**: TLS 1.2+ for all PHI transmission
- **Email**: Encrypted email (S/MIME, PGP) or secure portal

### Access Control
- Unique user IDs (no shared accounts)
- Role-based access control (RBAC)
- Automatic logoff after inactivity (15-30 minutes)
- Emergency access procedures

### Authentication
- Strong passwords (8+ characters, complexity)
- Multi-factor authentication (MFA) for remote access
- Token-based or biometric authentication

### Audit Logging
- Log all PHI access (view, print, export, modify)
- Retention: Minimum 6 years
- Regular log review for anomalies
- Immutable logs (tamper-proof)

### Backup and Disaster Recovery
- Regular backups (daily minimum)
- Offsite backup storage (encrypted)
- Disaster recovery plan tested annually
- Recovery Time Objective (RTO) and Recovery Point Objective (RPO) defined

---

## Business Associate Agreement (BAA)

Required between covered entities and vendors handling PHI. Must include:
- Permitted uses and disclosures of PHI
- Safeguards to protect PHI
- Breach notification obligations
- Return or destruction of PHI upon termination
- Liability and indemnification

**Cloud Providers with BAA**:
- AWS (HIPAA-eligible services)
- Microsoft Azure (HIPAA compliance)
- Google Cloud (HIPAA compliance)
- Salesforce Health Cloud

---

## HIPAA Compliance Checklist

**Administrative**:
- [ ] Conduct annual risk assessment
- [ ] Designate Privacy Officer and Security Officer
- [ ] Implement security policies and procedures
- [ ] Train workforce on HIPAA annually
- [ ] Execute Business Associate Agreements (BAAs)
- [ ] Develop incident response plan

**Physical**:
- [ ] Control facility access (badges, biometrics)
- [ ] Secure workstations and devices
- [ ] Implement visitor logs and escort policy
- [ ] Secure disposal of PHI (shredding, degaussing)

**Technical**:
- [ ] Encrypt PHI at rest and in transit
- [ ] Implement unique user IDs and authentication
- [ ] Enable audit logging (6-year retention)
- [ ] Automatic session timeouts
- [ ] Implement firewall and IDS/IPS
- [ ] Regular vulnerability scans and patching

**Breach Notification**:
- [ ] Breach notification procedures documented
- [ ] 60-day notification timeline established
- [ ] HHS breach reporting portal access
- [ ] Media notification process (500+ individuals)

---

## Resources

- [HHS HIPAA Portal](https://www.hhs.gov/hipaa/index.html)
- [HIPAA Security Rule](https://www.hhs.gov/hipaa/for-professionals/security/index.html)
- [Breach Notification Rule](https://www.hhs.gov/hipaa/for-professionals/breach-notification/index.html)
