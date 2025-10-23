# How the Cloud Architecture Expansion Pack Works

## What is a BMAD Expansion Pack?

An expansion pack is a **collection of specialized agents, tasks, templates, and knowledge** that extends BMAD core for a specific domain. Think of it like a plugin or add-on.

**BMAD Core** provides:
- Software development agents (analyst, pm, architect, dev, qa, etc.)
- Core workflows (PRD creation, story development, testing)
- General-purpose tasks and templates

**Cloud Architecture Expansion Pack** adds:
- Cloud-specific agents (infrastructure-analyst, cloud-architect, cost-optimizer, security-reviewer)
- Cloud workflows (requirements → design → cost → security)
- Cloud knowledge (patterns, best practices, compliance, services)

## Project Structure

```
bmad-architecture-agent/
│
├── .bmad-core/                    # Core BMAD (installed via npm)
│   ├── agents/                    # 10 core agents
│   ├── core-config.yaml          # ← Declares expansion pack usage
│   └── [tasks, templates, etc.]
│
└── expansion-packs/
    └── cloud-architecture/        # Your expansion pack
        ├── pack-config.yaml       # ← Expansion pack metadata
        ├── agents/                # Cloud agents
        ├── tasks/                 # Cloud tasks
        ├── templates/             # Cloud templates
        └── data/                  # Cloud knowledge
```

## How It's Connected

### 1. Declaration in core-config.yaml

```yaml
# .bmad-core/core-config.yaml
expansionPacks:
  - cloud-architecture          # ← Tells BMAD to load this pack
```

This line registers the expansion pack with BMAD.

### 2. Expansion Pack Metadata

```yaml
# expansion-packs/cloud-architecture/pack-config.yaml
pack:
  id: cloud-architecture
  name: Cloud Architecture Design & Analysis
  version: 1.0.0

agents:
  - infrastructure-analyst
  - cloud-architect
  - cost-optimizer
  - security-reviewer
```

This defines what the expansion pack contains.

## How Agents Are Accessed

### Method 1: Direct File Read (What Actually Happens)

When you use a slash command:

```
/cloud-analyst
```

The slash command file says:
```markdown
Read the file: expansion-packs/cloud-architecture/agents/infrastructure-analyst.md
```

**That's it!** Claude reads the markdown file and adopts that persona.

### Method 2: File Path Resolution

You can also directly tell Claude:
```
Read: expansion-packs/cloud-architecture/agents/cloud-architect.md
```

Claude reads the file, sees the YAML config, and becomes that agent.

## The Mechanics: How an Agent Works

Let's trace what happens when you activate an agent:

### Step 1: Activation

```
User: /cloud-analyst
```

### Step 2: File Read

Claude Code executes the slash command which says:
```
Read: expansion-packs/cloud-architecture/agents/infrastructure-analyst.md
```

### Step 3: Claude Reads the Agent File

```markdown
<!-- Powered by BMAD™ Core -->

# infrastructure-analyst

CRITICAL: Read the full YAML BLOCK...

```yaml
agent:
  name: Alex
  id: infrastructure-analyst
  title: Infrastructure Analyst

persona:
  role: Cloud Infrastructure Requirements Analyst
  style: Thorough, analytical...

commands:
  - help: Show numbered list...
  - analyze: Execute requirements analysis...

dependencies:
  tasks:
    - analyze-requirements.md
  templates:
    - requirements-template.yaml
  data:
    - patterns/microservices.md
```
```

### Step 4: Claude Adopts Persona

Claude reads this and understands:
- "I am Alex, an Infrastructure Analyst"
- "I have these commands available"
- "I have access to these tasks, templates, and data files"
- "I should greet the user and show *help"

### Step 5: Agent Response

```
Hello! I'm Alex, your Cloud Infrastructure Requirements Analyst.

I specialize in:
- Requirements gathering and analysis
- Technical feasibility assessment
- Integration planning
- Risk identification

Type *help to see available commands.
```

### Step 6: User Issues Command

```
User: *analyze
```

### Step 7: Claude Executes Task

Claude sees the command `analyze` maps to the task `analyze-requirements.md`:

```yaml
commands:
  - analyze: Execute requirements analysis using analyze-requirements task
```

Claude then:
1. Knows to look in `expansion-packs/cloud-architecture/tasks/analyze-requirements.md`
2. Reads that task file
3. Follows the workflow defined in the task
4. References templates and data as needed

### Step 8: Task Execution

From `tasks/analyze-requirements.md`:
```markdown
# Requirements Analysis Task

## Task Flow

### Step 1: Project Context
First, understand the project context:
1. Business Objectives: What business problems are we solving?
2. Success Criteria: How will we measure success?
...
```

Claude follows these instructions step by step, asking you questions and gathering information.

### Step 9: Knowledge Reference

When the task says "Reference architecture patterns," Claude:
1. Knows from dependencies it has access to `data/patterns/`
2. Can read specific files like `data/patterns/microservices.md`
3. Incorporates that knowledge into responses

### Step 10: Output Creation

When complete, Claude writes to:
```
docs/requirements.md
```

This becomes the input/context for the next agent.

## Dependency Resolution

### How Dependencies Work

In the agent file:
```yaml
dependencies:
  tasks:
    - analyze-requirements.md        # Path: tasks/analyze-requirements.md
  templates:
    - requirements-template.yaml    # Path: templates/requirements-template.yaml
  data:
    - patterns/microservices.md      # Path: data/patterns/microservices.md
```

When Claude needs a dependency:
```
IDE-FILE-RESOLUTION:
  - Dependencies map to {root}/{type}/{name}
  - type=folder (tasks|templates|data), name=file-name
  - Example: analyze-requirements.md → expansion-packs/cloud-architecture/tasks/analyze-requirements.md
```

Claude constructs the full path:
- `tasks/analyze-requirements.md` → `expansion-packs/cloud-architecture/tasks/analyze-requirements.md`
- `data/patterns/microservices.md` → `expansion-packs/cloud-architecture/data/patterns/microservices.md`

Then reads those files when needed.

## Cross-Agent Context Handoff

### Workflow Sequence

```
Infrastructure Analyst (Alex)
  ↓ writes docs/requirements.md
  
Cloud Architect (Morgan)
  ↓ activation reads docs/requirements.md
  ↓ writes docs/architecture.md
  
Cost Optimizer (Taylor)
  ↓ activation reads docs/architecture.md
  ↓ writes docs/cost-analysis.md
  
Security Reviewer (Jordan)
  ↓ activation reads all previous docs
  ↓ writes docs/security-assessment.md
```

Each agent:
1. Reads outputs from previous agents (context preservation)
2. Does its specialized work
3. Writes its output
4. Next agent picks up and continues

## The "Magic" - It's Just Markdown!

**There's no code execution.** The entire system is:

1. **Markdown files** that define agent personas and workflows
2. **Claude reading** those files and adopting the personas
3. **File-based context** (docs/ folder) for handoffs
4. **YAML metadata** to structure agent behavior

That's why we could move away from the Python implementation - BMAD doesn't need code execution. Claude IS the execution engine.

## Expansion Pack Benefits

### Isolation
```
.bmad-core/              # Core BMAD (don't modify)
expansion-packs/         # Your specializations (modify freely)
```

You can:
- Update core BMAD without affecting your agents
- Share your expansion pack independently
- Install multiple expansion packs

### Modularity

```yaml
expansionPacks:
  - cloud-architecture        # Cloud agents
  - devops-automation         # Could add more
  - infrastructure-as-code    # Could add more
```

Each pack is self-contained.

### Reusability

Someone else can:
```bash
git clone your-repo
cd bmad-architecture-agent
npm install
# They have your expansion pack ready to use
/cloud-analyst
```

## What Makes It an "Expansion Pack"?

1. **Location**: In `expansion-packs/` directory
2. **Configuration**: Has `pack-config.yaml` with metadata
3. **Registration**: Declared in `.bmad-core/core-config.yaml`
4. **Structure**: Follows BMAD conventions (agents/, tasks/, templates/, data/)
5. **Independence**: Can be used alone or with core BMAD agents

## Integration with Core BMAD

You can use **both** core BMAD agents and expansion pack agents:

### Scenario 1: Pure Cloud Architecture Workflow
```
/cloud-analyst       # Expansion pack
/cloud-architect     # Expansion pack
/cloud-cost          # Expansion pack
/cloud-security      # Expansion pack
```

### Scenario 2: Full Software + Cloud Workflow
```
# Use core BMAD for software planning
[Read .bmad-core/agents/analyst.md]     # Core: Business requirements
[Read .bmad-core/agents/pm.md]          # Core: Product plan

# Use expansion pack for cloud infrastructure
/cloud-architect     # Expansion: Cloud design
/cloud-cost          # Expansion: Cost analysis

# Back to core BMAD for development
[Read .bmad-core/agents/dev.md]         # Core: Implementation
[Read .bmad-core/agents/qa.md]          # Core: Testing
```

### Scenario 3: Core Architect + Cloud Specialists
```
[Read .bmad-core/agents/architect.md]   # Core: Software architecture
/cloud-architect     # Expansion: Cloud infrastructure
/cost-optimizer      # Expansion: Cost validation
```

The expansion pack **extends**, not replaces, core BMAD.

## Summary

**The expansion pack is just a folder of markdown files** that:

1. Lives in `expansion-packs/cloud-architecture/`
2. Is registered in `.bmad-core/core-config.yaml`
3. Contains agents that Claude reads directly
4. Provides specialized agents, tasks, templates, and knowledge for cloud architecture
5. Works alongside (or independently from) core BMAD agents

**No special integration code needed.** Claude reads the markdown files, adopts the personas, and follows the workflows. That's the beauty of BMAD - it's prompt engineering, not code engineering.
