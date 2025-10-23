# BMAD Cloud Architecture Expansion Pack

A comprehensive cloud architecture design and analysis expansion pack for the **BMAD-METHOD** (Building Multi-Agent Designs) framework.

## What is BMAD?

**BMAD-METHOD** is a universal AI agent framework that provides structured, reusable prompts and workflows for AI-assisted work across any domain. Unlike code-based frameworks, BMAD agents are markdown files with YAML configuration that you interact with directly in your IDE or AI chat interface.

**Key Concepts:**
- **Agents**: Specialized AI personas defined in markdown files
- **Tasks**: Reusable workflow procedures
- **Templates**: Structured document formats
- **No Code Execution**: The AI (Claude/GPT/Gemini) IS the engine
- **Context Preservation**: Agents read from and write to `docs/` folder

Learn more: [BMAD-METHOD Repository](https://github.com/bmad-code-org/BMAD-METHOD)

## This Expansion Pack

This expansion pack adds specialized cloud architecture agents to BMAD:

- **Infrastructure Analyst** (Alex) - Requirements gathering and feasibility
- **Cloud Architect** (Morgan) - Architecture design and service selection
- **Cost Optimizer** (Taylor) - Cost analysis and optimization
- **Security Reviewer** (Jordan) - Security assessment and compliance

## Project Structure

```
bmad-architecture-agent/
├── .bmad-core/                    # BMAD core framework (installed via npm)
│   ├── agents/                    # Core BMAD agents (analyst, pm, architect, dev, qa, etc.)
│   ├── tasks/                     # Core tasks
│   ├── templates/                 # Core templates
│   └── core-config.yaml          # Project configuration
│
├── expansion-packs/
│   └── cloud-architecture/        # THIS EXPANSION PACK
│       ├── agents/                # Cloud-specific agents
│       │   ├── infrastructure-analyst.md
│       │   ├── cloud-architect.md
│       │   ├── cost-optimizer.md
│       │   └── security-reviewer.md
│       ├── tasks/                 # Cloud-specific tasks
│       ├── templates/             # Cloud-specific templates
│       ├── data/                  # Cloud services, compliance data
│       └── pack-config.yaml       # Expansion pack configuration
│
├── docs/                          # Generated artifacts (agents write here)
│   ├── requirements.md
│   ├── architecture.md
│   ├── cost-analysis.md
│   └── security-assessment.md
│
├── .claude/commands/              # Claude Code slash commands
│   ├── cloud-analyst.md
│   ├── cloud-architect.md
│   ├── cloud-cost.md
│   └── cloud-security.md
│
├── knowledge/                     # Reference materials (old structure, can keep for reference)
├── templates/                     # Old templates (archived)
└── agents/                        # Old agents (archived)
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

```
/cloud-analyst      # Start with Infrastructure Analyst
/cloud-architect    # Move to Cloud Architect
/cloud-cost         # Analyze costs
/cloud-security     # Security review
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

## Workflow Example

### Complete Cloud Architecture Design

**Step 1: Requirements Analysis**
```
/cloud-analyst
```
Agent greets you as Alex and shows commands. Then:
```
*analyze
```
Follow the interactive prompts to gather requirements. Output saved to `docs/requirements.md`.

**Step 2: Architecture Design**
```
/cloud-architect
```
Agent greets you as Morgan. The agent automatically reads `docs/requirements.md`. Then:
```
*design
```
Follow prompts to design the solution. Output saved to `docs/architecture.md`.

**Step 3: Cost Analysis**
```
/cloud-cost
```
Agent greets you as Taylor. The agent reads `docs/architecture.md`. Then:
```
*analyze
```
Provides cost projections. Output saved to `docs/cost-analysis.md`.

**Step 4: Security Review**
```
/cloud-security
```
Agent greets you as Jordan. The agent reads all previous docs. Then:
```
*review
```
Performs security assessment. Output saved to `docs/security-assessment.md`.

**Done!** You now have complete architecture documentation in `docs/`.

## Agent Commands

Each agent has commands (use `*` prefix):

### Infrastructure Analyst
- `*analyze` - Requirements analysis
- `*assess-feasibility` - Technical feasibility
- `*create-requirements-doc` - Generate requirements document
- `*help` - Show all commands
- `*exit` - Exit agent

### Cloud Architect
- `*design` - Architecture design
- `*select-services` - Cloud service selection
- `*create-architecture-doc` - Generate architecture document
- `*compare-platforms` - AWS vs Azure vs GCP comparison
- `*help` - Show all commands
- `*exit` - Exit agent

### Cost Optimizer
- `*analyze` - Cost analysis
- `*calculate-tco` - Total Cost of Ownership
- `*create-cost-report` - Generate cost report
- `*optimize` - Optimization recommendations
- `*help` - Show all commands
- `*exit` - Exit agent

### Security Reviewer
- `*review` - Security review
- `*assess-compliance` - Compliance validation (GDPR, HIPAA, etc.)
- `*threat-model` - Threat modeling
- `*create-security-assessment` - Generate security report
- `*help` - Show all commands
- `*exit` - Exit agent

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

## Comparison: Old vs New Implementation

### What We Had Before (Python Framework)

- Python orchestration code (`bmad/core/`)
- API calls to Claude
- Programmatic agent execution
- File-based but with Python layer

### What We Have Now (True BMAD)

- Pure markdown agents with YAML config
- No code execution - AI reads prompts directly
- Native IDE integration (Claude Code, Cursor)
- Can bundle for web UI (ChatGPT, Gemini)
- Part of BMAD ecosystem

### Why This is Better

1. **True BMAD Compliance**: Follows official BMAD-METHOD framework
2. **No Dependencies**: Just markdown files, no Python required
3. **IDE Native**: Works directly in Claude Code without API calls
4. **Portable**: Same agents work in IDE or web UI
5. **Maintainable**: Update prompts, not code
6. **Extensible**: Create new agents easily
7. **Community**: Part of larger BMAD ecosystem

## Old Python Implementation

The previous Python orchestration framework is archived in:
- `bmad/` - Python code (archived, not needed)
- `examples/` - Python examples (archived)
- `bmad-*.md` slash commands (replaced)

You can safely ignore or delete these. The BMAD framework doesn't need them.

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
