# BMAD Knowledge Base - Usage Example

This shows how agents reference and use the knowledge base in practice.

## Scenario: Designing a Microservices Architecture

### Step 1: Activate Cloud Architect

```
/cloud-architect
```

Agent response:
```
Hello! I'm Morgan, your Cloud Solutions Architect.
I have access to architecture patterns, best practices, and cloud service catalogs.

Type *help to see available commands.
```

### Step 2: Request Architecture Design

```
*design
```

### Step 3: Agent Uses Knowledge Internally

The agent will reference knowledge files during the design process:

**Agent's internal process:**
1. Reads `data/patterns/microservices.md` for pattern guidance
2. Reviews `data/best-practices/scalability.md` for scaling considerations
3. Consults `data/best-practices/security.md` for security patterns
4. References `data/services/aws-services.yaml` for service options

**Agent's response:**
```
I'll design a microservices architecture based on best practices.

Based on the microservices pattern guidelines, I recommend:

1. Service Architecture:
   - API Gateway for routing
   - Independent service deployments
   - Container orchestration (ECS/EKS)

2. Scalability (from scalability best practices):
   - Auto-scaling groups per service
   - Horizontal scaling strategy
   - Load balancing across instances

3. Service Selection (from AWS services catalog):
   - Amazon EKS for container orchestration
   - Application Load Balancer
   - Amazon RDS for databases
   - ElastiCache for caching

[Agent continues with detailed design...]
```

### Step 4: Agent References Specific Knowledge

If you ask for specifics:

```
User: What security patterns should we follow?
```

Agent response:
```
Let me reference the security best practices...

According to our security guidelines (data/best-practices/security.md):

1. Defense in Depth:
   - Network segmentation via VPCs
   - Security groups and NACLs
   - WAF for application layer

2. Identity and Access:
   - IAM roles with least privilege
   - Service-to-service authentication
   - Secrets Manager for credentials

[Agent continues with specific recommendations...]
```

## How It Works

### Agent Dependencies Declaration

In `cloud-architect.md`:
```yaml
dependencies:
  data:
    - patterns/microservices.md
    - best-practices/scalability.md
    - best-practices/security.md
    - services/aws-services.yaml
```

### Task Execution

When the agent executes the `design-architecture` task:

```markdown
# Architecture Design Task

1. Review requirements
2. **Reference architecture patterns** from data/patterns/
3. **Apply best practices** from data/best-practices/
4. **Select services** from data/services/
5. Create detailed design
```

### Agent Behavior

The agent:
- **Knows** it has access to these files (via dependencies)
- **References** them when relevant to the task
- **Incorporates** knowledge into recommendations
- **Cites** sources when appropriate

## Example: Compliance Review

### Activate Security Reviewer

```
/cloud-security
```

### Request Compliance Assessment

```
*assess-compliance
```

### Agent Uses Compliance Knowledge

**Agent's internal process:**
1. Reads `data/compliance/gdpr.md` for GDPR requirements
2. Reviews `data/compliance/hipaa.md` if healthcare data mentioned
3. Checks `data/compliance/pci-dss.md` if payment data involved

**Agent's response:**
```
I'll assess compliance requirements based on your architecture.

GDPR Compliance (from compliance framework):
- ✓ Data encryption at rest and in transit
- ✓ Right to be forgotten (data deletion capabilities)
- ⚠ Need explicit consent management system
- ✓ Data processing agreement with cloud provider

HIPAA Compliance (from compliance framework):
- ✓ PHI encryption requirements met
- ✓ Access controls via IAM
- ⚠ Need Business Associate Agreement (BAA) with AWS
- ✓ Audit logging enabled

[Agent continues with detailed assessment...]
```

## Example: Cost Optimization

### Activate Cost Optimizer

```
/cloud-cost
```

### Request Optimization

```
*optimize
```

### Agent Uses Cost Best Practices

**Agent references:** `data/best-practices/cost-optimization.md`

**Agent's response:**
```
Based on cost optimization best practices, I identify these opportunities:

1. Right-Sizing (from cost optimization guidelines):
   - EC2 instances are oversized by 30%
   - Recommend t3.medium instead of m5.large
   - Potential savings: $2,400/year

2. Commitment Discounts (from cost optimization guidelines):
   - Steady-state workloads suitable for Reserved Instances
   - 3-year RI commitment: 54% savings
   - Potential savings: $18,000/year

[Agent continues with specific recommendations...]
```

## Manual Knowledge Reference

You can also explicitly ask agents to review knowledge:

```
User: Review the serverless pattern and tell me if it fits our needs

Agent: Let me review the serverless pattern documentation...
[Reads data/patterns/serverless.md]

Based on the serverless pattern:

Fits your needs:
✓ Variable workload (pattern recommends serverless for this)
✓ Event-driven architecture (perfect match)
✓ No server management preference

Doesn't fit:
✗ Need for persistent connections (pattern shows limitation)
✗ Long-running processes (15-min Lambda limit)

Recommendation: Hybrid approach...
```

## Key Points

1. **Knowledge is referenced**, not automatically injected
2. **Agents know** what knowledge they have access to (via dependencies)
3. **Task instructions** guide when to reference knowledge
4. **You can explicitly** ask agents to review specific files
5. **Knowledge informs** agent recommendations and decisions

## Updating Knowledge

When you update a knowledge file:

```bash
# Edit the file
vim expansion-packs/cloud-architecture/data/patterns/microservices.md

# Agents automatically use updated content on next activation
/cloud-architect
```

No code changes needed - just update the markdown files!

## Comparing to Automated RAG

**What we have (BMAD Native):**
- Manual, structured knowledge reference
- Agent knows what files exist
- References specific files when relevant
- Clear, traceable knowledge usage

**What we don't have (would require custom code):**
- Automatic semantic search
- Vector embeddings
- Automatic knowledge injection
- Similarity-based retrieval

For most architecture workflows, **manual reference is sufficient and more predictable**.
