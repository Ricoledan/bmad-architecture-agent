# Cloud Architecture Knowledge Base

This directory contains reference knowledge that agents can use during their work.

## Structure

```
data/
├── best-practices/       # Cloud architecture best practices
├── compliance/           # Compliance framework requirements
├── patterns/            # Architecture patterns and designs
└── services/            # Cloud service catalogs and comparisons
```

## How Agents Use This Knowledge

Agents reference these files in their dependencies and can load them when needed.

### Example Usage

When the Cloud Architect agent is designing a solution:

```
User: *design
Agent: "I'll design a microservices architecture. Let me reference the best practices..."
Agent: *internally references data/patterns/microservices.md*
Agent: *internally references data/best-practices/scalability.md*
Agent: "Based on microservices patterns, I recommend..."
```

### Available Knowledge

#### Architecture Patterns (`patterns/`)
- `microservices.md` - Microservices architecture pattern
- `serverless.md` - Serverless architecture pattern
- `event-driven.md` - Event-driven architecture pattern
- `data-lake.md` - Data lake pattern
- `multi-region.md` - Multi-region deployment pattern
- `hybrid-cloud.md` - Hybrid cloud architecture
- `three-tier.md` - Classic three-tier architecture

#### Best Practices (`best-practices/`)
- `cost-optimization.md` - Cloud cost optimization strategies
- `security.md` - Security best practices
- `scalability.md` - Scalability patterns
- `reliability.md` - Reliability and resilience
- `performance.md` - Performance optimization
- `monitoring.md` - Monitoring and observability

#### Compliance Frameworks (`compliance/`)
- `gdpr.md` - GDPR requirements
- `hipaa.md` - HIPAA requirements
- `pci-dss.md` - PCI-DSS requirements
- `sox.md` - SOX requirements
- `iso-27001.md` - ISO 27001 requirements

#### Cloud Services (`services/`)
- `aws-services.yaml` - AWS service catalog
- `azure-services.yaml` - Azure service catalog
- `gcp-services.yaml` - GCP service catalog
- `service-mapping.yaml` - Cross-cloud service equivalents

## Adding New Knowledge

To add new knowledge:

1. Create markdown files in appropriate subdirectory
2. Update agent dependencies to reference new files
3. Follow existing file structure and format

### Example File Format

```markdown
# Pattern/Topic Name

## Overview
Brief description...

## When to Use
Circumstances where this applies...

## Key Considerations
Important points to consider...

## Implementation Details
Specific guidance...

## Related Patterns/Practices
Links to related knowledge...
```

## Referencing in Agents

In agent files, declare data dependencies:

```yaml
dependencies:
  data:
    - patterns/microservices.md
    - best-practices/scalability.md
    - compliance/gdpr.md
```

Agents can then reference these during task execution.
