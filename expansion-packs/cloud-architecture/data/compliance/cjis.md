# CJIS Security Policy Compliance

## Overview

The **CJIS (Criminal Justice Information Services) Security Policy** establishes security requirements, guidelines, and agreements for federal, state, and local law enforcement agencies accessing Criminal Justice Information (CJI). Compliance is mandatory for any system accessing FBI databases (NCIC, N-DEx, etc.) or handling CJI.

**Issuing Authority**: FBI Criminal Justice Information Services Division
**Current Version**: CJIS Security Policy v5.9 (as of 2023)
**Applicability**: All agencies with access to FBI systems, contractors, cloud service providers
**Sensitivity Level**: Criminal Justice Information (CJI)

---

## What is CJI (Criminal Justice Information)?

CJI includes:
- **NCIC (National Crime Information Center)** data
- **III (Interstate Identification Index)** data
- **N-DEx (National Data Exchange)** information
- Biometric data (fingerprints, photos, DNA)
- Case files and incident reports
- Warrants and protection orders
- Criminal history records
- Personally Identifiable Information (PII) in criminal context

**CJI ≠ Public Information**: Mug shots on websites, publicly available court records, etc. are NOT CJI

---

## CJIS Policy Areas (13 Areas)

### Area 1: Information Exchange Agreements

**Requirements**:
- Signed **CJIS Security Addendum** between agency and FBI
- Contractor Security Addendums for third parties
- Cloud Service Provider (CSP) agreements
- Management Control Agreements for outsourced functions

**Technical Implementation**:
- Contract management system
- Addendum tracking and expiration alerts
- Vendor compliance validation
- Annual agreement renewal workflows

---

### Area 2: Security Awareness Training

**Requirements**:
- **All users** with access to CJI must complete CJIS Security Awareness Training
- **Frequency**: Annual minimum, ideally every 2 years
- **Content**: CJIS policy, CJI handling, security practices, incident response
- **Documentation**: Training completion certificates, attendance records

**Training Topics**:
- What is CJI
- Handling and dissemination restrictions
- Physical security
- Password management
- Incident reporting
- Social engineering awareness
- Mobile device security

**Technical Implementation**:
- Learning Management System (LMS)
- Training completion tracking
- Automated reminders for renewals
- Compliance reporting dashboard
- Quiz/assessment validation

---

### Area 3: Incident Response

**Requirements**:
- **Incident Response Plan** documented and tested
- **Incident Response Team** designated
- **Notification**: FBI CJIS within **1 hour** of confirmed breach
- **Types of Incidents**: Unauthorized access, data breach, malware, physical breach

**Incident Response Phases**:
1. **Preparation**: IR plan, team training, tools
2. **Detection & Analysis**: SIEM, IDS, user reports
3. **Containment**: Isolate affected systems
4. **Eradication**: Remove threat, patch vulnerabilities
5. **Recovery**: Restore systems, validate security
6. **Post-Incident**: Lessons learned, policy updates

**Notification Requirements**:
- **FBI CJIS ISO** (Information Security Officer) - within 1 hour
- **State CJIS Systems Officer** (CSO)
- **Affected individuals** (if PII compromised)
- **Other agencies** (if their data affected)

**Technical Implementation**:
- SIEM (Security Information and Event Management)
- Automated incident detection
- Incident ticketing system
- Notification automation
- Forensics and logging tools
- Communication templates

---

### Area 4: Auditing and Accountability

**Requirements**:
- **Audit logging** of all access to CJI
- **Retention**: Minimum **5 years** for audit logs
- **Log content**: User ID, date/time, action, data accessed, source IP
- **Log review**: Regular review of logs for suspicious activity
- **Immutable logs**: Append-only, tamper-proof

**Auditable Events**:
- User authentication (success/failure)
- CJI access (query, view, print, export)
- System configuration changes
- Privilege escalation
- Data modifications or deletions
- Account creation/modification/deletion
- Failed access attempts

**Technical Implementation**:
- Centralized log aggregation (Splunk, ELK, Azure Sentinel)
- Immutable log storage
- Real-time alerting on suspicious activity
- Automated log analysis
- Compliance reporting
- Log backup and archival

---

### Area 5: Access Control

**Requirements**:
- **Need-to-know basis**: Access only to CJI required for job function
- **Role-Based Access Control (RBAC)**
- **Multi-Factor Authentication (MFA)** for remote access
- **Unique user IDs**: No shared accounts
- **Account review**: Quarterly review of accounts and permissions
- **Immediate revocation**: Deactivate accounts upon separation

**Access Control Types**:
- **Physical access**: Badge systems, biometrics, visitor logs
- **Logical access**: Authentication, authorization, RBAC
- **Remote access**: VPN with MFA, encrypted connections
- **Mobile access**: MDM, encryption, remote wipe

**Password Requirements** (Advanced Authentication):
- Minimum 12 characters (14+ recommended)
- Complexity: Upper, lower, number, special character
- No dictionary words or personal information
- Password expiration: 90 days maximum
- Password history: Cannot reuse last 10 passwords
- Account lockout: 10 failed attempts
- MFA for remote access

**Technical Implementation**:
- Identity and Access Management (IAM) system
- RBAC enforcement
- MFA solution (Duo, Okta, Azure MFA)
- Password policy enforcement
- Privileged Access Management (PAM)
- Just-in-time (JIT) access

---

### Area 6: Identification and Authentication

**Requirements**:
- **Unique identifiers** for every user
- **Advanced Authentication** (passwords + controls OR MFA)
- **Biometric authentication** for high-security areas (optional but encouraged)
- **Session timeouts**: 30 minutes of inactivity
- **Token-based authentication** for APIs

**Authentication Methods**:
- Something you know (password, PIN)
- Something you have (token, smart card, phone)
- Something you are (fingerprint, facial recognition)

**Technical Implementation**:
- Single Sign-On (SSO) with MFA
- OAuth 2.0 / OpenID Connect
- SAML for federated identity
- Biometric readers
- Smart card/PIV card systems
- Session management

---

### Area 7: Configuration Management

**Requirements**:
- **Baseline configurations** for all systems
- **Change control process**: Documented, approved, tested
- **Vulnerability management**: Scan quarterly, patch within 30 days (critical vulnerabilities ASAP)
- **Asset inventory**: Maintain list of all hardware/software with CJI access
- **Security testing**: Annual penetration testing, vulnerability assessments

**Configuration Management Process**:
1. Baseline configuration documented
2. Change requested and approved
3. Testing in non-production environment
4. Change implemented with rollback plan
5. Validation and documentation

**Technical Implementation**:
- Configuration Management Database (CMDB)
- Change management system (ServiceNow, Jira)
- Vulnerability scanners (Nessus, Qualys, Rapid7)
- Patch management automation
- Infrastructure as Code (Terraform, Ansible)
- Compliance scanning tools

---

### Area 8: Media Protection

**Requirements**:
- **Encryption at rest**: All CJI on portable media (laptops, USB, phones)
- **Encryption standard**: FIPS 140-2 validated
- **Media sanitization**: DOD 5220.22-M wipe or physical destruction
- **Media tracking**: Log chain of custody
- **Secure disposal**: Shred or incinerate physical media

**Media Types**:
- Hard drives (HDD, SSD)
- USB drives and external storage
- Optical media (CD, DVD)
- Backup tapes
- Mobile devices
- Paper documents

**Sanitization Methods**:
- **Clear**: Logical overwrite (for reuse within organization)
- **Purge**: Advanced overwrite or degaussing (for reuse outside organization)
- **Destroy**: Physical destruction (shredding, incineration, disintegration)

**Technical Implementation**:
- Full-disk encryption (BitLocker, FileVault, LUKS)
- USB encryption (Apricorn, IronKey)
- Mobile Device Management (MDM) with encryption
- Certified data destruction services
- Media tracking system
- Certificate of Destruction documentation

---

### Area 9: Physical Protection

**Requirements**:
- **Controlled areas**: Physical barriers, badge access
- **Visitor management**: Sign-in, escort, badge
- **Security cameras**: Monitor CJI areas
- **Secure disposal**: Locked bins for paper shredding
- **Clean desk policy**: No CJI left unattended
- **Layered security**: Multiple physical barriers

**Physical Security Zones**:
- **Public Zone**: Lobby, waiting areas
- **Restricted Zone**: Office areas (badge required)
- **Secure Zone**: Server rooms, evidence lockers (additional controls)
- **High-Security Zone**: Biometric access, mantrap, 24/7 monitoring

**Technical Implementation**:
- Badge access systems
- Video surveillance (CCTV)
- Intrusion detection systems (IDS)
- Visitor management system
- Environmental monitoring (temp, humidity)
- Secure disposal containers

---

### Area 10: Systems and Communications Protection

**Requirements**:
- **Encryption in transit**: TLS 1.2+ for all CJI transmission
- **Encryption at rest**: FIPS 140-2 validated encryption
- **Network segmentation**: Separate CJI network from other networks
- **Firewalls**: Between zones, deny-by-default
- **Intrusion Detection/Prevention Systems (IDS/IPS)**
- **VPN with MFA** for remote access
- **Wireless security**: WPA3-Enterprise or equivalent

**Network Architecture**:
- **DMZ** (Demilitarized Zone) for public-facing services
- **CJI Network**: Isolated, encrypted, monitored
- **Management Network**: Separate for administration
- **Firewalls**: Between each zone
- **IDS/IPS**: Monitor all traffic

**Encryption Standards**:
- **At Rest**: AES-256
- **In Transit**: TLS 1.3 (TLS 1.2 minimum)
- **VPN**: IPsec or SSL VPN with MFA
- **Email**: S/MIME or PGP for CJI

**Technical Implementation**:
- Network segmentation (VLANs, subnets)
- Next-generation firewalls (Palo Alto, Fortinet)
- IDS/IPS (Snort, Suricata, Cisco Firepower)
- VPN concentrators (Cisco AnyConnect, OpenVPN)
- TLS certificate management
- Encryption key management (HSM)

---

### Area 11: Systems and Information Integrity

**Requirements**:
- **Anti-malware**: Deploy on all systems, update daily
- **Patch management**: Apply security patches within 30 days
- **System monitoring**: SIEM, log analysis, alerting
- **Integrity checking**: File integrity monitoring (FIM)
- **Input validation**: Prevent injection attacks
- **Error handling**: Don't expose sensitive info in errors

**Anti-Malware Requirements**:
- Real-time scanning
- Scheduled full scans (weekly)
- Automatic signature updates (daily)
- Quarantine malicious files
- Alert on detection

**Technical Implementation**:
- Endpoint protection (CrowdStrike, SentinelOne, Defender)
- Patch management (WSUS, SCCM, Ivanti)
- SIEM (Splunk, QRadar, Azure Sentinel)
- File Integrity Monitoring (Tripwire, OSSEC)
- Application security testing
- Penetration testing (annual)

---

### Area 12: Mobile Devices

**Requirements**:
- **MDM (Mobile Device Management)** required
- **Encryption**: Full-device encryption
- **Remote wipe**: Capability to remotely erase data
- **Strong authentication**: PIN/password + biometric
- **Automatic lock**: 2 minutes of inactivity
- **Jailbreak/root detection**
- **No CJI on personal devices** (unless approved BYOD policy)

**BYOD (Bring Your Own Device) Policy**:
- Container approach (separate work/personal data)
- MDM enrollment required
- Compliance checks
- Data loss prevention (DLP)
- Annual device security assessments

**Technical Implementation**:
- MDM solution (Microsoft Intune, VMware Workspace ONE, MobileIron)
- Containerization (Samsung Knox, Android Enterprise)
- Mobile Threat Defense (MTD)
- App whitelisting/blacklisting
- Conditional access policies
- Remote wipe workflows

---

### Area 13: Formally Documented Policies

**Requirements**:
- **Security Policy**: Comprehensive, approved, annually reviewed
- **Acceptable Use Policy (AUP)**: User responsibilities
- **Incident Response Plan**: Procedures, contacts, escalation
- **Disaster Recovery Plan**: Backup, recovery, business continuity
- **Personnel Security Policy**: Background checks, separation of duties
- **Training Policy**: Requirements, frequency, tracking

**Required Policy Documents**:
1. Information Security Policy
2. Acceptable Use Policy
3. Incident Response Plan
4. Access Control Policy
5. Password Policy
6. Encryption Policy
7. Remote Access Policy
8. Mobile Device Policy
9. Vendor Management Policy
10. Media Protection Policy
11. Physical Security Policy

**Policy Review Process**:
- **Frequency**: Annual minimum
- **Approver**: CISO or designee
- **Version control**: Track changes
- **Distribution**: All users acknowledge
- **Updates**: Document rationale

**Technical Implementation**:
- Policy management system
- Digital acknowledgment workflow
- Version control (Git, SharePoint)
- Policy review calendar
- Compliance tracking dashboard

---

## CJIS Certification and Compliance

### CJIS Certification Process

1. **Self-Assessment**: Use CJIS Security Policy checklist
2. **Triennial Audit**: Every 3 years by FBI or State CSO
3. **Remediation**: Address findings within specified timeframe
4. **Continuous Monitoring**: Ongoing compliance validation

### CJIS Audit Scope

**Areas Audited**:
- Information Exchange Agreements (current and valid)
- Training records (completion, currency)
- Incident response capability (plan, team, drills)
- Audit logs (retention, review, protection)
- Access controls (RBAC, MFA, account reviews)
- Authentication mechanisms
- Configuration management (baselines, change control)
- Media protection (encryption, sanitization)
- Physical security (access controls, monitoring)
- Encryption (at rest, in transit)
- Anti-malware and patching
- Mobile device management
- Policy documentation (current, approved, acknowledged)

### Non-Compliance Consequences

**Potential Actions**:
- Warning and corrective action plan
- Suspension of NCIC/III access
- Termination of access
- Legal action (for willful violations)
- Criminal penalties (unauthorized disclosure)

---

## Cloud Service Provider (CSP) Requirements

### CSP CJIS Compliance

Cloud providers hosting CJI must:
- Sign **FBI Cloud Computing Addendum**
- Implement all 13 CJIS policy areas
- Provide **FedRAMP Moderate** or higher authorization (preferred)
- Support **customer-managed encryption keys**
- Allow **customer audit** of security controls
- **US-based data centers** (FBI requirement)
- **US-based support staff** with background checks
- **Dedicated environments** (no multi-tenancy for CJI - optional but preferred)

### CJIS-Compliant Cloud Providers

**Major Cloud Providers with CJIS Compliance**:
- **AWS**: GovCloud, specific regions
- **Microsoft Azure**: Azure Government, dedicated regions
- **Google Cloud**: Assured Workloads for Government
- **Oracle Cloud**: Government Cloud

**Implementation Pattern**:
```
Agency <--> Cloud Provider (CJIS compliant)
  ├── Encryption: Customer-managed keys
  ├── Access: MFA, RBAC, audit logging
  ├── Network: Encrypted VPN, firewall
  ├── Data: US-based, segregated
  └── Audit: Annual CJIS audit
```

---

## Technical Architecture for CJIS Compliance

### Reference Architecture

```
┌─────────────────────────────────────────────────────────────┐
│ Physical Security Layer                                      │
│ - Badge access, cameras, secure areas                       │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│ Network Security Layer                                       │
│ - Firewalls, IDS/IPS, network segmentation                  │
│ - VPN with MFA for remote access                            │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│ Application Security Layer                                   │
│ - Authentication (MFA), authorization (RBAC)                 │
│ - Session management, input validation                      │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│ Data Security Layer                                          │
│ - Encryption at rest (AES-256), in transit (TLS 1.3)        │
│ - Audit logging, access controls                            │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│ Monitoring & Incident Response Layer                         │
│ - SIEM, log analysis, alerting                              │
│ - Incident response team, forensics                         │
└─────────────────────────────────────────────────────────────┘
```

---

## CJIS Compliance Checklist

### Pre-Deployment
- [ ] CJIS Security Addendum signed
- [ ] Contractor/CSP agreements in place
- [ ] Policies documented and approved
- [ ] Security architecture reviewed
- [ ] Encryption implemented (rest + transit)
- [ ] Network segmentation configured
- [ ] Access controls implemented (RBAC + MFA)

### Deployment
- [ ] Anti-malware deployed and updated
- [ ] Audit logging enabled and tested
- [ ] Firewall rules configured
- [ ] IDS/IPS deployed
- [ ] Backup and DR plan tested
- [ ] Physical security measures in place
- [ ] MDM configured for mobile devices

### Post-Deployment
- [ ] All users trained (CJIS Security Awareness)
- [ ] Accounts reviewed and validated
- [ ] Incident response plan tested
- [ ] Vulnerability scan completed
- [ ] Penetration test completed (annual)
- [ ] Audit logs reviewed regularly
- [ ] Triennial audit scheduled

### Ongoing Operations
- [ ] Quarterly access reviews
- [ ] Quarterly vulnerability scans
- [ ] Annual training renewals
- [ ] Annual policy reviews
- [ ] Annual penetration testing
- [ ] Patch management (30-day SLA)
- [ ] Continuous monitoring and alerting

---

## Resources

### Official Guidance
- [CJIS Security Policy (Current Version)](https://www.fbi.gov/services/cjis/cjis-security-policy-resource-center)
- [CJIS Security Policy FAQ](https://www.fbi.gov/services/cjis/cjis-security-policy-resource-center/cjis-security-policy-faqs)
- [FBI CJIS Division](https://www.fbi.gov/services/cjis)

### State CJIS Contacts
- Each state has a **CJIS Systems Officer (CSO)** - contact via state law enforcement agency

### Training
- [CJIS Security Awareness Training](https://www.fbi.gov/services/cjis/cjis-security-policy-resource-center) (Free from FBI)

### Cloud Provider CJIS Resources
- [AWS CJIS Compliance](https://aws.amazon.com/compliance/cjis/)
- [Azure CJIS Compliance](https://docs.microsoft.com/en-us/azure/compliance/offerings/offering-cjis)
- [Google Cloud CJIS](https://cloud.google.com/security/compliance/cjis)

---

## Summary

CJIS compliance is mandatory for law enforcement systems accessing CJI. Key requirements:

1. **Access Control**: MFA, RBAC, need-to-know
2. **Encryption**: FIPS 140-2 at rest and TLS in transit
3. **Audit Logging**: 5-year retention, immutable
4. **Incident Response**: 1-hour FBI notification
5. **Training**: Annual CJIS Security Awareness for all users
6. **Physical Security**: Controlled access, monitoring
7. **Mobile Devices**: MDM, encryption, remote wipe
8. **Policies**: Comprehensive, annually reviewed
9. **Patching**: 30-day SLA for security patches
10. **Triennial Audit**: FBI or State CSO audit every 3 years

Systems must implement **defense in depth** across all 13 CJIS policy areas to protect Criminal Justice Information.
