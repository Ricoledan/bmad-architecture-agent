---
description: Check the status of a BMAD workflow project
---

You are being asked to check the status of a BMAD workflow project.

## Your Task

1. Ask the user for the project ID they want to check (or list all projects if they're not sure)

2. To list all projects:

```python
import sys
sys.path.insert(0, '/Users/ricoledan/dev/projects/bmad-architecture-agent')

from bmad.core.orchestrator import Orchestrator

orchestrator = Orchestrator(
    agents_dir="agents",
    projects_dir="projects"
)

projects = orchestrator.list_projects()

for project in projects:
    print(f"\n{project['project_id']}")
    print(f"  Started: {project.get('started_at', 'Unknown')}")
    print(f"  Current: {project.get('current_phase', 'Complete')}")
    print(f"  Completed: {project.get('completed_phases', 0)} phase(s)")
```

3. To check a specific project status:

```python
import sys
sys.path.insert(0, '/Users/ricoledan/dev/projects/bmad-architecture-agent')

from bmad.core.orchestrator import Orchestrator

orchestrator = Orchestrator(
    agents_dir="agents",
    projects_dir="projects"
)

status = orchestrator.get_project_status("<PROJECT_ID>")

print(f"\nProject: {status['project_id']}")
print(f"Started: {status['started_at']}")
print(f"Current Phase: {status['current_phase'] or 'Complete'}")
print(f"Completed Phases: {', '.join(status['completed_phases']) if status['completed_phases'] else 'None'}")
print(f"\nMetadata:")
for key, value in status['metadata'].items():
    print(f"  {key}: {value}")
```

4. Offer to show specific agent outputs if the user wants to review them:

```python
from bmad.core.context import ContextManager

context_manager = ContextManager("projects")
context = context_manager.load_project("<PROJECT_ID>")
output = context_manager.get_agent_output(context, "<AGENT_NAME>")
print(output)
```

## Useful Information

- Project contexts are stored in `projects/<project-id>/`
- Agent outputs are saved as markdown files in `projects/<project-id>/outputs/`
- The workflow sequence for the architect agent is:
  1. infrastructure-analyst
  2. cloud-architect
  3. cost-optimizer
  4. security-reviewer
