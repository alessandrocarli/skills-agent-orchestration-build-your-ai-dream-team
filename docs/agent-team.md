# Agent team

For Mona's Project Pulse dashboard, I am using a four-agent custom team defined under the repository's agent folder at `.github/agents/`, orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 4.7 (copilot). Responsibility: research the codebase, review dependencies and edge cases, and produce a practical implementation plan with file assignments and validation expectations. Definition: `.github/agents/planner.agent.md`.
- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsibility: break the plan into phases, delegate work to the specialist agents, coordinate parallel vs. sequential execution, and verify the integrated result. Definition: `.github/agents/orchestrator.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implement the code changes, fix bugs, and handle runnable app support such as launch configuration for the Project Pulse dashboard. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsibility: shape the dashboard UX, visual hierarchy, accessibility, responsive layout, and the polished Project Pulse look and feel. Definition: `.github/agents/designer.agent.md`.

This setup keeps planning, orchestration, implementation, and design work clearly separated while using GitHub Copilot CLI in a Codespace as the control layer for the full project workflow.
