# FedRAMP High Baseline

## Overview

FedRAMP (Federal Risk and Authorization Management Program) is a U.S. government program that provides a standardized approach to security assessment, authorization, and continuous monitoring for cloud products and services.

**Issuing Authority**: U.S. General Services Administration (GSA)
**Based On**: NIST SP 800-53 Rev. 5
**Impact Levels**: Low, Moderate, High
**Applicability**: Cloud Service Providers (CSPs) serving federal agencies

---

## FedRAMP Impact Levels

| Level | Data Classification | Impact of Breach |
|-------|--------------------|--------------------|
| **Low** | Non-sensitive, publicly releasable | Limited effect |
| **Moderate** | Sensitive, non-classified | Serious adverse effect |
| **High** | Highly sensitive, law enforcement, emergency services | Severe or catastrophic adverse effect |

**FedRAMP High** is required for:
- Law enforcement systems
- Emergency services systems
- Financial systems
- Healthcare systems
- Systems with PII
- Systems with national security information

---

## FedRAMP High Baseline (421 Security Controls)

Based on NIST 800-53 High Baseline plus additional FedRAMP requirements.

### Control Families (20 Families)

1. **Access Control (AC)** - 25 controls
2. **Awareness and Training (AT)** - 5 controls
3. **Audit and Accountability (AU)** - 16 controls
4. **Assessment, Authorization, and Monitoring (CA)** - 9 controls
5. **Configuration Management (CM)** - 13 controls
6. **Contingency Planning (CP)** - 13 controls
7. **Identification and Authentication (IA)** - 11 controls
8. **Incident Response (IR)** - 10 controls
9. **Maintenance (MA)** - 6 controls
10. **Media Protection (MP)** - 8 controls
11. **Physical and Environmental Protection (PE)** - 20 controls
12. **Planning (PL)** - 9 controls
13. **Program Management (PM)** - 16 controls
14. **Personnel Security (PS)** - 8 controls
15. **Risk Assessment (RA)** - 10 controls
16. **System and Services Acquisition (SA)** - 23 controls
17. **System and Communications Protection (SC)** - 51 controls
18. **System and Information Integrity (SI)** - 23 controls
19. **Supply Chain Risk Management (SR)** - 12 controls
20. **Privacy (P)** - 8 controls

---

## Key FedRAMP High Requirements

### Multi-Factor Authentication (MFA)
- **Required** for all users (privileged and non-privileged)
- **Phishing-resistant MFA** required for privileged users
- PIV/CAC cards or FIDO2 tokens

### Encryption
- **At Rest**: FIPS 140-2 Level 2 (minimum)
- **In Transit**: TLS 1.2+ with FIPS-approved cipher suites
- **Key Management**: HSM with FIPS 140-2 Level 2+

### Audit Logging
- Comprehensive logging of all security-relevant events
- **Retention**: Minimum 1 year online, 3 years total
- **Immutable logs**: Tamper-proof, append-only
- **Time synchronization**: NTP with authoritative source
- **Log review**: Automated tools + manual review

### Incident Response
- **Detection**: 24/7 monitoring and alerting
- **Notification**: US-CERT within 1 hour of confirmed incident
- **Root cause analysis**: Required for all incidents
- **Lessons learned**: Document and remediate

### Continuous Monitoring
- **POA&M (Plan of Action & Milestones)**: Track remediation of findings
- **Monthly continuous monitoring deliverables** to FedRAMP PMO
- **Annual assessment**: Full security assessment
- **Significant change**: Re-assessment required

### Vulnerability Management
- **Scanning**: Monthly authenticated scans minimum
- **Critical vulnerabilities**: Remediate within 30 days
- **High vulnerabilities**: Remediate within 90 days
- **Penetration testing**: Annual

### Separation of Duties
- **Development** ≠ **Production**: Separate environments and access
- **Developer** cannot deploy to production
- **Change approval** separate from implementation

### Data Location
- **US-based data centers** for FedRAMP High
- **US Citizen** support personnel for High systems
- **No offshoring** of data or operations

---

## FedRAMP Authorization Process

### 1. Preparation Phase
- Select impact level (Low, Moderate, High)
- Implement NIST 800-53 controls
- Develop System Security Plan (SSP)
- Select Third-Party Assessment Organization (3PAO)

### 2. Assessment Phase (by 3PAO)
- Security assessment testing
- Penetration testing
- Vulnerability scanning
- Security Assessment Report (SAR) produced

### 3. Authorization Phase
**Two Paths**:

**Agency Authorization**:
- Agency reviews SSP, SAR, POA&M
- Agency issues Authority to Operate (ATO)
- Listed on FedRAMP Marketplace
- Other agencies can leverage ATO

**Joint Authorization Board (JAB) Provisional ATO**:
- Review by DoD, DHS, GSA
- More rigorous, but more valuable
- Provisional ATO (P-ATO) issued
- Agencies can use with minimal additional review

### 4. Continuous Monitoring Phase
- Monthly POA&M updates
- Quarterly vulnerability scans
- Annual penetration testing
- Annual assessment
- Ongoing authorization (no expiration if maintained)

---

## FedRAMP Required Documentation

1. **System Security Plan (SSP)** - 200+ pages typically
   - System description, architecture, data flows
   - Control implementation details
   - Responsibility matrix (CSP vs. Customer)

2. **Security Assessment Report (SAR)** - by 3PAO
   - Testing results
   - Findings and vulnerabilities
   - Risk ratings

3. **Plan of Action & Milestones (POA&M)**
   - Open findings
   - Remediation plans
   - Target completion dates

4. **Continuous Monitoring Reports**
   - Monthly scans and assessment
   - Incident reports
   - Significant changes

5. **Incident Response Plan**

6. **Configuration Management Plan**

7. **Contingency Plan (DR/BCP)**

8. **Rules of Behavior** - User responsibilities

9. **Privacy Impact Assessment (PIA)**

10. **Control Implementation Summary (CIS)**

11. **Customer Responsibility Matrix (CRM)**

---

## FedRAMP High vs Moderate Differences

| Aspect | Moderate | High |
|--------|----------|------|
| **Controls** | 325 controls | 421 controls |
| **MFA** | Privileged users | All users |
| **Encryption** | FIPS 140-2 Level 1 | FIPS 140-2 Level 2 |
| **Data Location** | Can be international (with approval) | Must be US-based |
| **Personnel** | Background checks | US Citizens, stricter checks |
| **Monitoring** | Monthly reporting | Monthly + enhanced monitoring |
| **Incident Response** | 1-hour notification | 1-hour + enhanced procedures |
| **Cost** | $250K - $500K initial | $500K - $1M+ initial |
| **Timeline** | 6-12 months | 12-18 months |

---

## Cloud Provider FedRAMP Offerings

### AWS
- **AWS GovCloud (US)**: FedRAMP High authorized
- **AWS US East/West**: FedRAMP Moderate authorized
- **Services**: 100+ services FedRAMP High authorized

### Microsoft Azure
- **Azure Government**: FedRAMP High authorized
- **Office 365 GCC High**: FedRAMP High
- **Dynamics 365 Government**: FedRAMP High

### Google Cloud
- **Google Cloud for Government**: FedRAMP High authorized
- **Google Workspace Government**: FedRAMP High

---

## Cost and Timeline

### Initial Authorization
- **3PAO Assessment**: $150K - $300K
- **Remediation**: $200K - $500K
- **Documentation**: $100K - $200K
- **Total Initial Cost**: $500K - $1M+
- **Timeline**: 12-18 months

### Ongoing Costs
- **Annual 3PAO Re-assessment**: $100K - $200K
- **Continuous Monitoring**: $50K - $100K/year
- **POA&M Remediation**: Variable
- **Total Annual Cost**: $200K - $400K/year

---

## Common Challenges

1. **Documentation Burden**: SSP 200+ pages, constant updates
2. **Control Implementation**: 421 controls to implement and test
3. **Continuous Monitoring**: Monthly deliverables to FedRAMP PMO
4. **POA&M Management**: Tracking and remediating findings
5. **Cost**: Significant investment ($500K-$1M+ initial)
6. **Timeline**: 12-18 months for High authorization
7. **Separation of Duties**: Architectural changes required
8. **Audit Logging**: Comprehensive logging requirements
9. **Data Residency**: US-only requirement

---

## FedRAMP High Checklist

**Access Control**:
- [ ] MFA for all users (PIV/CAC for privileged)
- [ ] RBAC with least privilege
- [ ] Quarterly access reviews
- [ ] Separation of duties enforced

**Encryption**:
- [ ] FIPS 140-2 Level 2 encryption at rest
- [ ] TLS 1.2+ for all data in transit
- [ ] HSM for key management

**Audit & Logging**:
- [ ] Comprehensive audit logging
- [ ] 1-year online retention, 3-year total
- [ ] Immutable logs
- [ ] Automated log analysis + manual review

**Vulnerability Management**:
- [ ] Monthly vulnerability scans
- [ ] 30-day remediation for critical
- [ ] 90-day remediation for high
- [ ] Annual penetration testing

**Incident Response**:
- [ ] 24/7 monitoring and detection
- [ ] US-CERT notification within 1 hour
- [ ] Incident response plan tested annually

**Continuous Monitoring**:
- [ ] Monthly POA&M updates
- [ ] Monthly continuous monitoring reports
- [ ] Annual security assessment
- [ ] Significant change process

**Data Location**:
- [ ] US-based data centers only
- [ ] US citizen support staff
- [ ] No data offshoring

---

## Resources

- [FedRAMP.gov](https://www.fedramp.gov/)
- [FedRAMP Marketplace](https://marketplace.fedramp.gov/)
- [NIST SP 800-53 Rev. 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)
- [FedRAMP Templates](https://www.fedramp.gov/documents-templates/)
- [3PAO List](https://www.fedramp.gov/assessors/)
