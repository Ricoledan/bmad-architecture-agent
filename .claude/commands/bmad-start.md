---
description: Start a new BMAD workflow project with the architecture agent
---

You are being asked to start a new BMAD (Building Multi-Agent Designs) architecture workflow.

## Your Task

1. **Activate the BMAD Orchestrator** by reading the orchestrator agent file:
   ```
   Read: .bmad-core/agents/bmad-orchestrator.md
   ```

2. **Follow the orchestrator's activation instructions** which will:
   - Adopt the BMad Orchestrator persona
   - Load the project configuration from `.bmad-core/core-config.yaml`
   - Greet the user and show available commands with `*help`

3. **Guide the user** through the architecture workflow by suggesting:
   - Start with `/cloud-analyst` for requirements gathering
   - Then `/cloud-architect` for infrastructure design
   - Then `/data-architect` for database/graph design
   - Then `/integration-architect` for API design
   - Then `/platform-engineer` for deployment
   - Then `/cloud-cost` for cost analysis
   - Then `/cloud-security` for security review
   - Finally `/architecture-governor` for ADRs and governance

4. **Explain the workflow**:
   - Each agent writes outputs to `docs/` folder
   - Next agents read previous outputs for context
   - This creates an audit trail and workflow continuity
   - User can save outputs to a project-specific directory

## Alternative: Direct Agent Sequence

If the user wants to bypass the orchestrator and run agents directly, you can activate each agent sequentially using the slash commands above.
