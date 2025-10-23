# Government Audit Logging Requirements

## Overview

Comprehensive audit logging is a critical requirement across all government compliance frameworks (CJIS, FedRAMP, ICD 203, NIST 800-53). This guide consolidates audit logging requirements for government and law enforcement systems.

---

## What to Log (Security-Relevant Events)

### Authentication Events
- Successful logins
- Failed login attempts
- Account lockouts
- Password changes/resets
- MFA enrollment/use
- Session establishment/termination
- Privilege escalation

### Data Access Events
- View/read sensitive data (CJI, PHI, PII, classified)
- Create/modify/delete data
- Export/download data
- Print sensitive documents
- Copy to removable media
- Email/share sensitive data

### System Events
- System startup/shutdown
- Service start/stop
- Configuration changes
- Software installation/removal
- Patch application
- Backup/restore operations

### Administrative Events
- User account creation/modification/deletion
- Permission/role changes
- Security policy changes
- Audit log access
- Security control modifications

### Security Events
- Firewall rule changes
- IDS/IPS alerts
- Anti-malware detections
- Vulnerability scan results
- Security incidents
- Failed access attempts (authorization)

### Application Events
- Business-critical transactions
- Error conditions
- API calls (esp. sensitive operations)
- Batch job execution
- Report generation

---

## Required Log Fields

### Minimum Log Content
1. **Timestamp**: Date and time (with timezone)
2. **User ID**: Who performed the action
3. **Source IP/Hostname**: Where the action originated
4. **Action**: What was done (view, modify, delete, etc.)
5. **Resource**: What was accessed (file, record, system)
6. **Outcome**: Success or failure
7. **Reason for Failure**: If applicable

###Extended Log Content (for High-Security Systems)
8. **Session ID**: Link related actions
9. **Process ID**: Identify application/service
10. **Classification Level**: For classified data access
11. **Data Elements**: Specific fields accessed
12. **Before/After Values**: For modifications
13. **Geolocation**: Physical location of user
14. **Device ID**: Specific device used

---

## Log Retention Requirements

| Framework | Retention Period |
|-----------|------------------|
| **CJIS** | 5 years minimum |
| **FedRAMP High** | 1 year online, 3 years total |
| **HIPAA** | 6 years minimum |
| **SOX** | 7 years minimum |
| **ICD 203** | 5 years recommended |
| **NIST 800-53** | 1 year online, organization-defined total |

**Best Practice**: 7 years total retention (aligns with SOX)

---

## Log Protection Requirements

### Immutability (Tamper-Proof)
- Append-only logs (no modification/deletion)
- Cryptographic hashing (integrity verification)
- Write-once media or WORM storage
- Separate logging infrastructure

### Access Control
- Restricted access to logs (security team only)
- Separate permissions for log access vs. system admin
- MFA for log access
- Logs should not contain full PAN, SSN (mask/redact)

### Encryption
- **At Rest**: Encrypted log storage
- **In Transit**: TLS for log transmission
- **Key Management**: Separate encryption keys for logs

### Backup
- Regular backups of logs
- Offsite backup storage
- Test log restoration

---

## Log Review Requirements

### Frequency
- **Critical Systems**: Daily review (automated + manual spot-check)
- **Moderate Systems**: Weekly review
- **General Systems**: Monthly review

### Automated Analysis
- SIEM (Security Information and Event Management)
- Real-time alerting on suspicious activity
- Correlation of events across systems
- Baseline and anomaly detection

### Manual Review
- Spot-check automated analysis
- Review high-risk accounts (admin, privileged)
- Review failed access attempts
- Review after-hours access

### Alert Thresholds (Examples)
- Multiple failed logins (5+ within 15 minutes)
- Privilege escalation
- Access to classified/sensitive data from unusual location
- Large data exports
- Configuration changes outside change windows
- Disabled audit logging

---

## Time Synchronization

### Requirements
- **All systems synchronized** to authoritative time source
- **Accuracy**: Within 1 second of authoritative source
- **Protocol**: NTP (Network Time Protocol)
- **Source**: NIST, US Naval Observatory, GPS, or local stratum 1/2

### Implementation
- Primary and secondary NTP servers
- Firewall rules to allow NTP traffic
- Monitor time drift
- Log time sync events

---

## Framework-Specific Requirements

### CJIS Audit Logging
- All CJI access logged
- 5-year retention
- Immutable logs
- Regular review
- Incident notification within 1 hour

### FedRAMP High Audit Logging
- Comprehensive logging per NIST 800-53 AU family
- 1-year online, 3-year total retention
- Automated analysis with SIEM
- Monthly continuous monitoring reports
- Annual review of logging practices

### ICD 203 / Intelligence Community
- All classified data access logged
- Source and confidence level tracking
- Reasoning and assumptions logged
- Alternative analysis documentation
- 5-year retention recommended

### HIPAA Audit Logging
- All PHI access logged (view, print, export, modify)
- 6-year retention
- Audit controls (technical safeguard)
- Regular log review
- Breach detection

### SOX Audit Logging
- All changes to financial data logged
- 7-year retention
- Immutable logs
- Segregation of duties monitoring
- Change management compliance

---

## Log Aggregation and SIEM

### Centralized Logging Architecture
```
[Application Servers] ──> [Log Forwarder] ──> [SIEM] ──> [Long-Term Storage]
[Network Devices]     ──>                                      ↓
[Security Tools]      ──>                              [Archive/Backup]
[Cloud Services]      ──>
```

### Popular SIEM Solutions
- **Splunk**: Enterprise, government-focused
- **Azure Sentinel**: Cloud-native, government cloud available
- **IBM QRadar**: Enterprise SIEM
- **LogRhythm**: Security and compliance focus
- **Elastic (ELK Stack)**: Open-source option
- **Sumo Logic**: Cloud-native

### Log Forwarders/Agents
- **Fluentd**: Open-source, flexible
- **Filebeat**: Lightweight, Elastic ecosystem
- **Splunk Universal Forwarder**: Splunk ecosystem
- **Syslog-ng**: Traditional syslog
- **Azure Monitor Agent**: Azure native

---

## Common Log Formats

### CEF (Common Event Format)
```
CEF:0|Vendor|Product|Version|SignatureID|Name|Severity|Extension
```

### Syslog (RFC 5424)
```
<Priority>Version Timestamp Hostname AppName ProcID MsgID StructuredData Message
```

### JSON
```json
{
  "timestamp": "2025-01-23T14:30:00Z",
  "user": "john.doe",
  "source_ip": "192.168.1.100",
  "action": "view_record",
  "resource": "case_12345",
  "classification": "SECRET",
  "outcome": "success"
}
```

### Windows Event Log
- Event ID, Source, Category, Level
- Collected via Windows Event Forwarding or agents

---

## Audit Logging Checklist

**Log Content**:
- [ ] Timestamp, user, source IP, action, resource, outcome
- [ ] Authentication events (success/fail)
- [ ] Data access (view, modify, delete, export)
- [ ] Administrative actions
- [ ] Security events (alerts, policy changes)
- [ ] Application errors and exceptions

**Log Protection**:
- [ ] Immutable logs (append-only)
- [ ] Restricted access to logs
- [ ] Encryption at rest and in transit
- [ ] Regular backups

**Log Retention**:
- [ ] Retention period defined per framework (5-7 years)
- [ ] Online storage (1 year minimum)
- [ ] Archive storage for remainder
- [ ] Tested restoration process

**Log Review**:
- [ ] SIEM deployed for automated analysis
- [ ] Real-time alerting configured
- [ ] Daily/weekly/monthly manual review
- [ ] Incident response integration

**Time Synchronization**:
- [ ] NTP configured on all systems
- [ ] Authoritative time source used
- [ ] Time drift monitoring

**Compliance**:
- [ ] Framework-specific requirements met (CJIS, FedRAMP, etc.)
- [ ] Audit logs available for compliance audits
- [ ] Log review documentation maintained

---

## Resources

- [NIST SP 800-92: Guide to Computer Security Log Management](https://csrc.nist.gov/publications/detail/sp/800-92/final)
- [NIST SP 800-53 AU (Audit and Accountability) Controls](https://csrc.nist.gov/Projects/risk-management/sp800-53-controls/release-search#!/controls?version=5.1&family=AU)
- [CJIS Security Policy - Section 5.4 (Auditing and Accountability)](https://www.fbi.gov/services/cjis/cjis-security-policy-resource-center)
- [CIS Critical Security Controls v8](https://www.cisecurity.org/controls/)
