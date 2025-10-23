---
description: Run a single BMAD agent without full workflow orchestration
---

You are being asked to run a single BMAD agent.

## Your Task

1. Ask the user which agent they want to run:
   - `architect` - Master Cloud Architecture Orchestrator
   - `infrastructure-analyst` - Infrastructure Analysis Expert
   - `cloud-architect` - Cloud Solution Design Expert
   - `cost-optimizer` - Cost Analysis and Optimization Expert
   - `security-reviewer` - Security and Compliance Expert

2. Ask for:
   - **Task**: What they want the agent to do
   - **Project ID** (optional): If they want to save output to an existing project
   - **Context** (optional): Any relevant context or information

3. Execute the agent:

```python
import sys
sys.path.insert(0, '/Users/ricoledan/dev/projects/bmad-architecture-agent')

from bmad.core.orchestrator import Orchestrator

orchestrator = Orchestrator(
    agents_dir="agents",
    projects_dir="projects"
)

# Run single agent
output = orchestrator.execute_single_agent(
    agent_name="<AGENT_NAME>",
    task="<USER_TASK>",
    context="<CONTEXT_IF_PROVIDED>",
    project_id="<PROJECT_ID_IF_PROVIDED>"
)

print(output)
```

4. Display the agent's output to the user

## Available Agents

- **architect**: Orchestrates multi-agent workflows, provides high-level architectural guidance
- **infrastructure-analyst**: Analyzes requirements, assesses technical feasibility
- **cloud-architect**: Designs cloud solutions, selects services and patterns
- **cost-optimizer**: Analyzes costs, provides optimization recommendations
- **security-reviewer**: Reviews security and compliance requirements

## Notes

- Running a single agent does NOT execute the full workflow
- If a project ID is provided, the output will be saved for potential use in a workflow later
- For full workflow orchestration, use `/bmad-start` instead
