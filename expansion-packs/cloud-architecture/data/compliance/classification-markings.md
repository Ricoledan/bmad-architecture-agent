# Classification Markings and Handling

## Overview

Classification markings identify the level of protection required for national security information and criminal justice information. Proper marking ensures information is handled, stored, and transmitted according to its sensitivity level.

**Note**: For detailed ICD 203 marking requirements, see [icd-203.md](./icd-203.md)

---

## U.S. Classification Levels

### TOP SECRET (TS)
**Unauthorized disclosure could cause EXCEPTIONALLY GRAVE DAMAGE to national security**

Examples:
- Intelligence sources and methods
- Cryptographic systems and keys
- War plans and strategic operations
- Weapons of mass destruction intelligence

**Marking**: `TOP SECRET` or `TS`

---

### SECRET (S)
**Unauthorized disclosure could cause SERIOUS DAMAGE to national security**

Examples:
- Military operational plans
- Intelligence reports on foreign capabilities
- Detailed weapon system specifications
- Diplomatic communications

**Marking**: `SECRET` or `S`

---

### CONFIDENTIAL (C)
**Unauthorized disclosure could cause DAMAGE to national security**

Examples:
- Routine operational information
- Force movements
- Logistics plans
- Basic intelligence assessments

**Marking**: `CONFIDENTIAL` or `C`

---

### UNCLASSIFIED (U)
**Information that doesn't meet criteria for classification**

May still require protection under other categories:
- **CUI (Controlled Unclassified Information)**
- **FOUO (For Official Use Only)** - being phased out
- **LES (Law Enforcement Sensitive)**

**Marking**: `UNCLASSIFIED` or `U`

---

## Document Marking Requirements

### Banner Markings

**Top of Every Page**:
```
TOP SECRET//[SCI Compartments]//[Dissem Controls]//[FGI]
```

**Bottom of Every Page**:
```
TOP SECRET
```

### Portion Markings

**Every paragraph, title, subject line, bullet must be marked**:
```
(TS) This paragraph contains top secret information.
(S) This paragraph contains secret information.
(C) This paragraph is confidential.
(U) This is unclassified information.
```

**Images, tables, charts**: Mark with classification in caption or on image

### Classification Authority Block (Footer)

```
CLASSIFIED BY: [Name and Position or "Derived From"]
DERIVED FROM: [Source Document or Classification Guide]
DECLASSIFY ON: [Date or Event]
```

**Example**:
```
CLASSIFIED BY: John Smith, Chief Analyst
DERIVED FROM: NSC-2024-001
DECLASSIFY ON: 20350101
```

Or for derived classification:
```
CLASSIFIED BY: Derived from multiple sources
DERIVED FROM: NSA Report 2024-123, CIA Memo 2024-456
DECLASSIFY ON: 20400101
```

---

## Dissemination Controls

### NOFORN (Not Releasable to Foreign Nationals)
**Cannot be shared with foreign governments or foreign nationals**

Marking: `//NOFORN` or `//NF`

Example: `SECRET//NOFORN`

---

### REL TO (Releasable To)
**May be released to specified countries only**

Marking: `//REL TO USA, [COUNTRY CODES]`

Example: `SECRET//REL TO USA, GBR, CAN, AUS, NZL` (Five Eyes)

Common country codes:
- USA - United States
- GBR - United Kingdom
- CAN - Canada
- AUS - Australia
- NZL - New Zealand
- NATO - North Atlantic Treaty Organization

---

### FVEY (Five Eyes)
**Releasable to Five Eyes partners** (USA, UK, Canada, Australia, New Zealand)

Marking: `//FVEY`

Example: `SECRET//FVEY`

---

### ORCON (Originator Controlled)
**Further dissemination requires originator's permission**

Marking: `//ORCON`

Example: `TOP SECRET//ORCON//NOFORN`

---

### PROPIN (Proprietary Information)
**Contains proprietary or commercial information**

Marking: `//PROPIN`

---

### IMCON (Images Controlled)
**Applies to controlled imagery**

Marking: `//IMCON`

---

## SCI (Sensitive Compartmented Information)

SCI requires additional compartment markings. Access requires specific clearance and indoctrination.

### Common SCI Compartments
- **SI**: Special Intelligence (SIGINT)
- **TK**: Talent Keyhole (Imagery/GEOINT)
- **HCS**: HUMINT Control System
- **KDK**: Klondike (Geospatial)

**Marking**: `//SCI//SI//TK//HCS`

**Example**: `TOP SECRET//SCI//SI//TK//NOFORN`

---

## SAP (Special Access Programs)

Highly classified programs with additional security requirements.

**Marking**: `//SAP-[PROGRAM NICKNAME]//`

**Example**: `TOP SECRET//SAP-ALPHA//SI//NOFORN`

**Note**: Actual SAP nicknames are classified; "ALPHA" is illustrative only

---

## CUI (Controlled Unclassified Information)

Unclassified information requiring safeguarding or dissemination controls.

### CUI Categories (Examples)
- **CUI//SP-PRVCY**: Privacy information
- **CUI//SP-PROCURE**: Procurement sensitive
- **CUI//LEGAL**: Legal matters
- **CUI//LAW**: Law enforcement sensitive

### CUI Limited Dissemination Controls
- **FED ONLY**: Federal employees only
- **NOCON**: No contractors
- **LES**: Law enforcement sensitive

**Example**: `CUI//SP-PRVCY//FED ONLY`

---

## Law Enforcement Sensitive (LES)

Unclassified information related to law enforcement investigations.

**Markings**:
- `LAW ENFORCEMENT SENSITIVE` or
- `CUI//LES` or
- `CUI//LAW ENFORCEMENT SENSITIVE`

**Examples**:
- Active investigation details
- Investigative techniques
- Informant information
- Evidence before trial

---

## FOUO (For Official Use Only)

**Being phased out in favor of CUI**

Legacy marking for unclassified information with limited distribution.

**Marking**: `FOR OFFICIAL USE ONLY` or `FOUO`

---

## Marking Examples

### Example 1: Mixed Classification Memo
```
TOP SECRET//SI//NOFORN

MEMORANDUM FOR: Director of Intelligence

(U) SUBJECT: Threat Assessment Update

(U) Executive Summary

(S//NF) Intelligence indicates that adversary has developed new capabilities...

(TS//SI//NF) SIGINT reporting from [REDACTED SOURCE] confirms...

(U) Recommendations

(C) Continue monitoring through existing channels.

SECRET//NOFORN

CLASSIFIED BY: Jane Analyst, Senior Intelligence Officer
DERIVED FROM: Multiple IC Reporting, NSA-2024-456, CIA-2024-789
DECLASSIFY ON: 20450101
```

---

### Example 2: Law Enforcement Sensitive Report
```
LAW ENFORCEMENT SENSITIVE

INVESTIGATIVE REPORT

Case Number: 2024-CASE-12345

Subject: Investigation into organized crime activity

This report contains information related to an ongoing criminal investigation.
Unauthorized disclosure could compromise investigative techniques and
endanger confidential sources.

Distribution limited to law enforcement personnel with an operational need
to know.

LAW ENFORCEMENT SENSITIVE
```

---

### Example 3: CUI with Multiple Controls
```
CUI//SP-PRVCY//SP-LAW//LES

INTELLIGENCE INFORMATION REPORT (IIR)

(CUI) This report contains personally identifiable information (PII) and
law enforcement sensitive information related to criminal activity.

(CUI//LES) Subject of investigation: John Doe, DOB: 1985-03-15, SSN: XXX-XX-1234

(CUI//SP-PRVCY) Personal information includes home address, phone numbers,
and financial records obtained through lawful investigative means.

Distribution: Law enforcement personnel only with operational need to know.

CUI//SP-PRVCY//SP-LAW//LES
```

---

## Digital Document Marking

### Email Subject Lines
Always mark subject line with highest classification:

`Subject: (S//NF) Intelligence Update for 23 January 2025`

### File Names
Do NOT include classification in filename (filenames often leak metadata):

❌ **Bad**: `secret_intelligence_report.docx`
✅ **Good**: `intelligence_report_20250123.docx` (mark content, not filename)

### Metadata
- Remove or sanitize metadata before downgrading/releasing
- Track classification in document management systems

### Watermarks
- Optional but recommended for high classifications
- `TOP SECRET//NOFORN` watermark on each page

---

## Handling Requirements by Classification

| Classification | Storage | Transmission | Destruction |
|---------------|---------|--------------|-------------|
| **TOP SECRET** | GSA-approved safe/vault, SCIF | Encrypted, approved channels, courier | Burn, pulp, shred (NSA-approved shredder) |
| **SECRET** | GSA-approved safe, locked room | Encrypted, approved channels | Crosscut shred, burn |
| **CONFIDENTIAL** | Locked container | Encrypted when outside secure facility | Shred, burn |
| **CUI** | Locked when unattended | Encrypt for email | Shred or delete securely |

---

## Physical Markings

### Cover Sheets
Use **SF 703** (TOP SECRET), **SF 704** (SECRET), **SF 705** (CONFIDENTIAL) cover sheets

### Labels and Tags
- Use classification labels on binders, folders, storage media
- Color coding (optional):
  - TOP SECRET: Orange
  - SECRET: Red
  - CONFIDENTIAL: Blue

---

## Common Mistakes to Avoid

1. **Missing portion markings** - Every paragraph must be marked
2. **Incorrect highest classification** - Banner must reflect highest portion
3. **Missing dissemination controls** - NOFORN, REL TO, etc.
4. **Improper declassification guidance** - Must include declassify date/event
5. **Mixing classifications incorrectly** - Use proper format for multiple controls
6. **Filename classification** - Don't put classification in filename
7. **Email forwards** - Re-mark if classification changes
8. **Printing** - Print only in authorized locations
9. **Screenshots** - Must retain classification markings
10. **Storage** - Never store classified on unclassified systems

---

## Automated Marking Tools

### Document Classification Tools
- **Microsoft AIP (Azure Information Protection)**: Auto-labeling, DLP
- **Titus Classification Suite**: Automated classification
- **Boldon James Classifier**: Email and document marking
- **Forcepoint DLP**: Data loss prevention with classification
- **Varonis**: Data classification and access governance

### Email Classification
- **Microsoft Purview**: Email classification and DLP
- **Proofpoint**: Email security with classification
- **Mimecast**: Secure email gateway with marking

---

## Violations and Penalties

### Security Violations
- **Spills**: Classified information on unclassified system
- **Unauthorized disclosure**: Sharing with unauthorized persons
- **Improper storage**: Not securing classified material
- **Improper destruction**: Using unapproved method

### Consequences
- Administrative action (suspension of clearance)
- Termination of employment
- Criminal prosecution (willful violations)
- Civil penalties
- Imprisonment (up to 10 years for TS)

---

## Resources

### Official Guidance
- [Executive Order 13526](https://www.archives.gov/isoo/policy-documents/cnsi-eo.html) - Classified National Security Information
- [32 CFR Part 2002](https://www.ecfr.gov/current/title-32/subtitle-B/chapter-XX/part-2002) - CUI Regulation
- [ICD 203](https://www.dni.gov/files/documents/ICD/ICD%20203%20Analytic%20Standards.pdf) - Analytic Standards (marking requirements)
- [Marking Classified Information](https://www.archives.gov/isoo/policy-documents/marking-classified-national-security-info.html) - ISOO Guide

### Training
- Classification Management Training
- Derivative Classification Training
- CUI Training
- OPSEC (Operations Security) Training

---

## Checklist for Proper Marking

**Before Creating/Modifying Document**:
- [ ] Determine overall classification level
- [ ] Identify all dissemination controls
- [ ] Verify authority to classify/derive

**Document Markings**:
- [ ] Banner marking on top of every page
- [ ] Footer marking on bottom of every page
- [ ] Portion markings on all paragraphs, titles, bullets
- [ ] Classification authority block (footer)
- [ ] Cover sheet (if physical document)

**Dissemination**:
- [ ] Verify recipient has appropriate clearance
- [ ] Verify recipient has need-to-know
- [ ] Use approved transmission method
- [ ] Track dissemination (ORCON if required)

**Storage**:
- [ ] Store in appropriate container/facility
- [ ] Track in accountability system (for TS)
- [ ] Secure when unattended

**Destruction**:
- [ ] Use approved destruction method
- [ ] Witness destruction (for TS)
- [ ] Document destruction
- [ ] Certificate of Destruction (if required)
