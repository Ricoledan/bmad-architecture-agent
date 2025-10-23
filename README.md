# BMAD Architecture Suite

A complete expert architect toolkit with 5 specialized expansion packs for the **BMAD-METHOD** (Building Multi-Agent Designs) framework, covering cloud infrastructure, data architecture, API integration, platform engineering, and governance.

## What is BMAD?

**BMAD-METHOD** is a universal AI agent framework that provides structured, reusable prompts and workflows for AI-assisted work across any domain. Unlike code-based frameworks, BMAD agents are markdown files with YAML configuration that you interact with directly in your IDE or AI chat interface.

**Key Concepts:**
- **Agents**: Specialized AI personas defined in markdown files
- **Tasks**: Reusable workflow procedures
- **Templates**: Structured document formats
- **No Code Execution**: The AI (Claude/GPT/Gemini) IS the engine
- **Context Preservation**: Agents read from and write to `docs/` folder

Learn more: [BMAD-METHOD Repository](https://github.com/bmad-code-org/BMAD-METHOD)

## Architecture Suite Overview

This suite provides 5 expansion packs with 8 specialized architecture agents covering all aspects of expert architecture work:

### Cloud Infrastructure (cloud-architecture)
- **Infrastructure Analyst** (Alex) - Requirements gathering and feasibility
- **Cloud Architect** (Morgan) - Cloud infrastructure design and service selection
- **Cost Optimizer** (Taylor) - Cost analysis and optimization
- **Security Reviewer** (Jordan) - Security assessment and compliance

### Data Architecture (data-architecture)
- **Data Architect** (Casey) - Database design, data modeling, ETL/ELT pipelines, data governance

### Integration Patterns (integration-patterns)
- **Integration Architect** (River) - API design (REST, GraphQL, gRPC), messaging, event-driven architecture

### Platform Engineering (platform-engineering)
- **Platform Engineer** (Sam) - CI/CD pipelines, Kubernetes, Infrastructure as Code, GitOps, observability

### Architecture Governance (architecture-governance)
- **Architecture Governor** (Quinn) - ADRs, architecture reviews, tech radar, standards, technical debt

## Project Structure

```
bmad-architecture-agent/
├── .bmad-core/                    # BMAD core framework (installed via npm)
│   ├── agents/                    # Core BMAD agents (analyst, pm, architect, dev, qa)
│   ├── tasks/                     # Core tasks
│   ├── templates/                 # Core templates
│   └── core-config.yaml          # Project configuration
│
├── expansion-packs/
│   ├── cloud-architecture/        # Cloud Infrastructure Pack
│   │   ├── agents/                # Alex, Morgan, Taylor, Jordan
│   │   ├── tasks/                 # Cloud-specific tasks
│   │   ├── templates/             # Cloud templates
│   │   ├── data/                  # Cloud services, compliance data
│   │   └── pack-config.yaml
│   │
│   ├── data-architecture/         # Data Architecture Pack
│   │   ├── agents/                # Casey (Data Architect)
│   │   ├── tasks/                 # Database, pipeline, migration tasks
│   │   ├── templates/             # ERD, data dictionary, pipeline specs
│   │   ├── data/                  # Database patterns, best practices
│   │   └── pack-config.yaml
│   │
│   ├── integration-patterns/      # Integration Pack
│   │   ├── agents/                # River (Integration Architect)
│   │   ├── tasks/                 # API, messaging, event-driven tasks
│   │   ├── templates/             # API specs, event schemas
│   │   ├── data/                  # API patterns, protocols
│   │   └── pack-config.yaml
│   │
│   ├── platform-engineering/      # Platform Engineering Pack
│   │   ├── agents/                # Sam (Platform Engineer)
│   │   ├── tasks/                 # CI/CD, Kubernetes, IaC tasks
│   │   ├── templates/             # Pipeline, K8s, IaC templates
│   │   ├── data/                  # DevOps patterns, best practices
│   │   └── pack-config.yaml
│   │
│   └── architecture-governance/   # Governance Pack
│       ├── agents/                # Quinn (Architecture Governor)
│       ├── tasks/                 # ADR, review, tech radar tasks
│       ├── templates/             # ADR, review, standards templates
│       ├── data/                  # Governance frameworks, standards
│       └── pack-config.yaml
│
├── docs/                          # Generated artifacts (agents write here)
│   ├── requirements.md            # Infrastructure Analyst
│   ├── architecture.md            # Cloud Architect
│   ├── data-architecture.md       # Data Architect
│   ├── integration.md             # Integration Architect
│   ├── platform.md                # Platform Engineer
│   ├── cost-analysis.md           # Cost Optimizer
│   ├── security-assessment.md     # Security Reviewer
│   ├── governance.md              # Architecture Governor
│   └── architecture/
│       ├── decisions/             # ADRs
│       ├── tech-radar.md          # Technology radar
│       ├── standards/             # Coding standards
│       └── reviews/               # Architecture reviews
│
└── .claude/commands/              # Claude Code slash commands
    ├── cloud-analyst.md           # /cloud-analyst
    ├── cloud-architect.md         # /cloud-architect
    ├── cloud-cost.md              # /cloud-cost
    ├── cloud-security.md          # /cloud-security
    ├── data-architect.md          # /data-architect
    ├── integration-architect.md   # /integration-architect
    ├── platform-engineer.md       # /platform-engineer
    └── architecture-governor.md   # /architecture-governor
```

## Quick Start

### Prerequisites

- Node.js 20+ (for BMAD installation)
- Claude Code, Cursor, or other AI-powered IDE
- OR use in ChatGPT/Gemini web UI

### Installation

1. **Install BMAD Core**:
   ```bash
   npm install bmad-method
   npx bmad-method install
   ```
   This installs the `.bmad-core/` directory with all core BMAD agents and infrastructure.

2. **The expansion pack is already here** in `expansion-packs/cloud-architecture/`

3. **Verify installation**:
   ```bash
   ls .bmad-core/agents/  # Should see core BMAD agents
   ls expansion-packs/cloud-architecture/agents/  # Should see cloud agents
   ```

## Usage

### Method 1: Claude Code Slash Commands (Easiest)

Use the slash commands to activate agents directly:

**Cloud Infrastructure:**
```
/cloud-analyst      # Requirements gathering (Alex)
/cloud-architect    # Cloud design (Morgan)
/cloud-cost         # Cost analysis (Taylor)
/cloud-security     # Security review (Jordan)
```

**Data & Integration:**
```
/data-architect     # Database & data pipelines (Casey)
/integration-architect  # APIs & messaging (River)
```

**Platform & Governance:**
```
/platform-engineer  # CI/CD & Kubernetes (Sam)
/architecture-governor  # ADRs & reviews (Quinn)
```

Each command:
1. Loads the agent file
2. Activates the agent persona
3. Shows available commands with `*help`
4. Waits for your input

### Method 2: Direct Agent Activation

Read the agent file directly:

```
Read: expansion-packs/cloud-architecture/agents/infrastructure-analyst.md
```

Then follow the activation instructions in the agent file.

### Method 3: Core BMAD Workflow Integration

If using the full BMAD software development workflow, these agents can supplement the planning phase:

1. Use core BMAD `analyst` and `pm` agents for product planning
2. Activate `cloud-architect` expansion agent for infrastructure planning
3. Continue with core BMAD `architect` for application architecture
4. Use `cost-optimizer` and `security-reviewer` for validation

## Workflow Examples

### Complete Architecture Design (Full Workflow)

**Step 1: Governance - Initial ADRs (Optional but recommended)**
```
/architecture-governor
*create-adr
```
Document key architectural decisions upfront. Creates ADRs in `docs/architecture/decisions/`.

**Step 2: Requirements Analysis**
```
/cloud-analyst
*analyze
```
Gather requirements. Output: `docs/requirements.md`.

**Step 3: Cloud Infrastructure Design**
```
/cloud-architect
*design
```
Design cloud architecture. Output: `docs/architecture.md`.

**Step 4: Data Architecture**
```
/data-architect
*design-schema
*design-pipeline
```
Design database and data pipelines. Output: `docs/data-architecture.md`.

**Step 5: Integration Design**
```
/integration-architect
*design-api
*design-messaging
```
Design APIs and integrations. Output: `docs/integration.md`.

**Step 6: Platform Engineering**
```
/platform-engineer
*design-pipeline
*design-kubernetes
```
Design CI/CD and deployment. Output: `docs/platform.md`.

**Step 7: Cost Analysis**
```
/cloud-cost
*analyze
```
Analyze costs. Output: `docs/cost-analysis.md`.

**Step 8: Security Review**
```
/cloud-security
*review
```
Security assessment. Output: `docs/security-assessment.md`.

**Step 9: Final Architecture Review**
```
/architecture-governor
*review-architecture
```
Comprehensive review. Output: `docs/architecture/reviews/`.

**Done!** Complete architecture documentation across all domains.

### Quick Cloud-Only Workflow

For cloud-focused projects:
```
/cloud-analyst → /cloud-architect → /cloud-cost → /cloud-security
```

### Data-Focused Workflow

For data engineering projects:
```
/cloud-analyst → /data-architect → /integration-architect → /platform-engineer
```

## Agent Commands

Each agent has commands (use `*` prefix):

### Infrastructure Analyst (Alex)
- `*analyze` - Requirements analysis
- `*assess-feasibility` - Technical feasibility
- `*create-requirements-doc` - Generate requirements document
- `*help`, `*exit`

### Cloud Architect (Morgan)
- `*design` - Architecture design
- `*select-services` - Cloud service selection
- `*create-architecture-doc` - Generate architecture document
- `*compare-platforms` - AWS vs Azure vs GCP comparison
- `*help`, `*exit`

### Data Architect (Casey)
- `*design-schema` - Database schema design
- `*design-pipeline` - ETL/ELT pipeline design
- `*design-migration` - Data migration strategy
- `*optimize-database` - Performance optimization
- `*create-erd` - Entity-Relationship Diagram
- `*help`, `*exit`

### Integration Architect (River)
- `*design-api` - Design REST, GraphQL, or gRPC API
- `*design-messaging` - Message queue/pub-sub architecture
- `*design-event-driven` - Event-driven system design
- `*create-api-spec` - Generate OpenAPI specification
- `*help`, `*exit`

### Platform Engineer (Sam)
- `*design-pipeline` - CI/CD pipeline design
- `*design-kubernetes` - Kubernetes architecture
- `*design-iac` - Infrastructure as Code strategy
- `*design-observability` - Monitoring and logging
- `*design-gitops` - GitOps workflow
- `*help`, `*exit`

### Cost Optimizer (Taylor)
- `*analyze` - Cost analysis
- `*calculate-tco` - Total Cost of Ownership
- `*optimize` - Cost optimization recommendations
- `*create-cost-report` - Generate cost report
- `*help`, `*exit`

### Security Reviewer (Jordan)
- `*review` - Security review
- `*assess-compliance` - Compliance validation (GDPR, HIPAA, etc.)
- `*threat-model` - Threat modeling
- `*create-security-assessment` - Generate security report
- `*help`, `*exit`

### Architecture Governor (Quinn)
- `*create-adr` - Architecture Decision Record
- `*review-architecture` - Comprehensive architecture review
- `*update-tech-radar` - Update technology radar
- `*define-standards` - Create coding/architecture standards
- `*assess-tech-debt` - Technical debt assessment
- `*help`, `*exit`

## Knowledge Base

The expansion pack includes a comprehensive knowledge base in `expansion-packs/cloud-architecture/data/`:

### Structure

- **`patterns/`** - Architecture patterns (microservices, serverless, event-driven, etc.)
- **`best-practices/`** - Best practices (cost, security, scalability, performance, etc.)
- **`compliance/`** - Compliance frameworks (GDPR, HIPAA, PCI-DSS, SOX, ISO 27001)
- **`services/`** - Cloud service catalogs (AWS, Azure, GCP)

### How Agents Use Knowledge

Agents reference knowledge files when executing tasks:

```
User: /cloud-architect
Agent: "Hello! I'm Morgan. I have access to architecture patterns and best practices."
User: *design
Agent: [References data/patterns/microservices.md and data/best-practices/scalability.md]
Agent: "Based on microservices patterns, I recommend..."
```

You can also explicitly ask:
```
User: Review the serverless pattern for our use case
Agent: [Reads data/patterns/serverless.md]
Agent: "The serverless pattern fits because..."
```

**See [KNOWLEDGE-USAGE-EXAMPLE.md](KNOWLEDGE-USAGE-EXAMPLE.md) for detailed examples.**

### Adding Knowledge

Simply add markdown files to the appropriate subdirectory:

```bash
# Add new pattern
echo "# New Pattern..." > expansion-packs/cloud-architecture/data/patterns/my-pattern.md

# Update agent dependencies
# Edit agent file to reference: data/patterns/my-pattern.md
```

No code changes needed - agents automatically reference updated files.

## Understanding BMAD Agents

### Agent Structure

Each agent is a markdown file with YAML configuration:

```yaml
agent:
  name: Alex
  id: infrastructure-analyst
  title: Infrastructure Analyst
  icon: 📊
  whenToUse: Use for requirements analysis...

persona:
  role: Cloud Infrastructure Requirements Analyst
  style: Thorough, analytical...
  core_principles:
    - Requirements First
    - Stakeholder Alignment

commands:
  - help: Show numbered list...
  - analyze: Execute requirements analysis...

dependencies:
  tasks:
    - analyze-requirements.md
  templates:
    - requirements-template.yaml
```

### How It Works

1. **You activate the agent** (via slash command or direct read)
2. **Agent adopts persona** defined in YAML config
3. **Agent loads dependencies** only when needed
4. **You issue commands** (e.g., `*analyze`)
5. **Agent executes tasks** following workflows
6. **Agent writes outputs** to `docs/` folder
7. **Next agent reads** previous outputs for context

### Context Preservation

BMAD agents preserve context through file-based handoffs:

```
Infrastructure Analyst
  ↓ writes docs/requirements.md
Cloud Architect
  ↓ reads requirements.md, writes docs/architecture.md
Cost Optimizer
  ↓ reads architecture.md, writes docs/cost-analysis.md
Security Reviewer
  ↓ reads all previous docs, writes docs/security-assessment.md
```

This creates an **audit trail** and allows **workflow resumption**.

## Configuration

Edit `.bmad-core/core-config.yaml` to customize:

```yaml
# Expansion Packs
expansionPacks:
  - cloud-architecture

# Document Locations
requirements:
  requirementsFile: docs/requirements.md
architecture:
  architectureFile: docs/architecture.md
costAnalysis:
  costAnalysisFile: docs/cost-analysis.md
security:
  securityAssessmentFile: docs/security-assessment.md
```

## Extending the Expansion Pack

### Add New Agents

Create `expansion-packs/cloud-architecture/agents/my-agent.md` following the BMAD agent format.

### Add New Tasks

Create `expansion-packs/cloud-architecture/tasks/my-task.md` with workflow instructions.

### Add New Templates

Create `expansion-packs/cloud-architecture/templates/my-template.yaml` with document structure.

### Add Cloud Service Data

Add service catalogs, pricing data, and compliance requirements to `expansion-packs/cloud-architecture/data/`.

## Complete Architecture Suite Summary

### What This Suite Provides

**8 Specialized Architecture Agents** covering:
- ✅ Requirements & feasibility analysis
- ✅ Cloud infrastructure design (AWS, Azure, GCP)
- ✅ Database design & data pipelines
- ✅ API design & system integration
- ✅ CI/CD, Kubernetes, Infrastructure as Code
- ✅ Cost analysis & optimization
- ✅ Security assessment & compliance
- ✅ Architecture governance & decision management

**Comprehensive Knowledge Base**:
- Cloud service catalogs (AWS, Azure, GCP)
- Architecture patterns (microservices, serverless, event-driven, data lakes)
- Best practices (security, scalability, performance, cost optimization)
- Compliance frameworks (GDPR, HIPAA, PCI-DSS, SOX, ISO 27001)
- Database patterns & data engineering
- API design patterns (REST, GraphQL, gRPC)
- DevOps & platform engineering patterns

**Output Documentation**:
- Requirements documents
- Architecture diagrams and specs
- ERDs and data dictionaries
- API specifications (OpenAPI)
- Pipeline and infrastructure configs
- Cost analysis reports
- Security assessments
- Architecture Decision Records (ADRs)
- Technology radar
- Standards documentation

### Real-World Use Cases

**Enterprise Application**: Full workflow from requirements → cloud → data → APIs → platform → cost → security → governance

**Data Platform**: Focus on data architecture, integration, and platform engineering

**API-First Product**: Requirements → integration architect → platform engineer → security

**Cloud Migration**: Requirements → cloud architect → data architect → platform engineer → cost → security

**Technical Debt Assessment**: Architecture governor reviews existing architecture, creates ADRs, assesses technical debt

## Documentation

- [BMAD-METHOD Documentation](https://github.com/bmad-code-org/BMAD-METHOD/tree/main/docs)
- [BMAD Core Architecture](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/core-architecture.md)
- [Creating Expansion Packs](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/expansion-packs.md)
- [BMAD User Guide](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/user-guide.md)

## Support

- [BMAD Discord Community](https://discord.gg/gk8jAdXWmj)
- [GitHub Issues](https://github.com/bmad-code-org/BMAD-METHOD/issues)
- [GitHub Discussions](https://github.com/bmad-code-org/BMAD-METHOD/discussions)

## License

MIT License - see LICENSE file for details.

## Acknowledgments

Built on the [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) framework by BMad Code, LLC.

BMAD™ and BMAD-METHOD™ are trademarks of BMad Code, LLC.
