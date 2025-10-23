---
description: Start a new BMAD workflow project with the architecture agent
---

You are being asked to start a new BMAD (Building Multi-Agent Designs) workflow.

## Your Task

1. Ask the user for the following information if not already provided:
   - **Project ID**: A unique identifier for this architecture project (e.g., "ecommerce-platform", "data-pipeline")
   - **Project Description**: Brief description of what they're building
   - **Requirements**: The initial requirements or problem statement

2. Once you have this information, execute the BMAD workflow using Python:

```python
import sys
sys.path.insert(0, '/Users/ricoledan/dev/projects/bmad-architecture-agent')

from bmad.core.orchestrator import Orchestrator

# Initialize orchestrator
orchestrator = Orchestrator(
    agents_dir="agents",
    projects_dir="projects"
)

# Start workflow
result = orchestrator.start_workflow(
    project_id="<PROJECT_ID>",
    workflow_agent="architect",  # The master architect agent
    initial_task="<USER_REQUIREMENTS>",
    metadata={
        "workflow_agent": "architect",
        "description": "<PROJECT_DESCRIPTION>"
    }
)

# Display results
if result.success:
    print(f"✓ Workflow completed successfully!")
    print(f"  Phases completed: {', '.join(result.completed_phases)}")
    print(f"\nOutputs saved to: projects/{result.project_id}/outputs/")
else:
    print(f"✗ Workflow failed: {result.error}")
```

3. After execution:
   - Show the user where outputs were saved
   - Offer to show them specific agent outputs
   - Suggest next steps (review outputs, continue workflow, etc.)

## Important Notes

- This will execute the full architecture workflow sequentially through all agents
- Each agent's output is saved to `projects/<project-id>/outputs/<agent-name>.md`
- The workflow uses file-based context, so previous agent outputs inform subsequent agents
- If the user wants to run just one agent instead of the full workflow, suggest using `/bmad-run` instead
