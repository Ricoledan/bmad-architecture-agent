---
description: Activate the Architecture Governor agent for ADRs and architecture reviews
---

You are being asked to activate the Architecture Governor agent from the BMAD Architecture Governance expansion pack.

## Instructions

1. Load the Architecture Governor agent file:
   ```
   Read the file: expansion-packs/architecture-governance/agents/architecture-governor.md
   ```

2. Follow the activation instructions in the agent file exactly as written

3. The agent will:
   - Greet you as Quinn, the Architecture Governor
   - Display available commands using `*help`
   - Wait for your requests

## Available Commands (use with * prefix)

- `*help` - Show all available commands
- `*create-adr` - Create Architecture Decision Record
- `*review-architecture` - Conduct comprehensive architecture review
- `*update-tech-radar` - Update technology radar with adoption recommendations
- `*define-standards` - Create or update coding and architecture standards
- `*assess-tech-debt` - Evaluate and prioritize technical debt
- `*conduct-design-review` - Review detailed design before implementation
- `*create-quality-report` - Generate architecture quality assessment
- `*exit` - Exit the agent

## When to Use This Agent

Use the Architecture Governor when you need to:
- Document architecture decisions (ADRs)
- Conduct architecture reviews
- Maintain technology radar
- Define coding and architecture standards
- Assess and prioritize technical debt
- Ensure quality attributes are met
- Facilitate decision-making processes

## Workflow Position

The Architecture Governor can be used at any point in the workflow:

**At the start:**
1. **Architecture Governor** - Create initial ADRs for key decisions
2. Infrastructure Analyst - Requirements gathering
3. [Continue workflow...]

**During design:**
- Create ADRs for major architectural decisions
- Conduct design reviews before implementation

**At the end:**
- Final architecture review
- Quality assessment
- Technical debt documentation

**Ongoing:**
- Maintain tech radar
- Update standards
- Review and update ADRs

## Input

The agent reviews all architecture documentation and decisions made throughout the project.

## Output

The agent creates:
- ADRs in `docs/architecture/decisions/`
- Architecture review reports in `docs/architecture/reviews/`
- Tech radar in `docs/architecture/tech-radar.md`
- Standards in `docs/architecture/standards/`
- Governance documentation in `docs/governance.md`
