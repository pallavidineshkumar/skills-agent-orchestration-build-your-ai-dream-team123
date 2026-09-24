# Agent team

The Mona's Project Pulse dashboard will be built by a coordinated team of four custom agents. GitHub Copilot CLI running in a Codespace orchestrates the work, delegates tasks to the specialists, and verifies the integrated result.

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the project, breaks the dashboard work into phases, assigns explicit file scopes, manages dependencies and parallel work, and verifies the integrated result. | [`.github/agents/orchestrator.agent.md`](../.github/agents/orchestrator.agent.md) |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository, documentation, dependencies, requirements, risks, and edge cases, then produces an implementation plan with assignments and validation expectations. | [`.github/agents/planner.agent.md`](../.github/agents/planner.agent.md) |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines and implements the dashboard's UI/UX, information hierarchy, accessibility, responsive behavior, visual styling, project cards, status badges, and priority treatment. | [`.github/agents/designer.agent.md`](../.github/agents/designer.agent.md) |
| **Coder** | GPT-5.5 (copilot) | Implements the assigned application logic and support configuration, follows repository patterns, keeps behavior deterministic and testable, and validates the changes. | [`.github/agents/coder.agent.md`](../.github/agents/coder.agent.md) |

The Orchestrator starts with the Planner, runs independent design and implementation work in parallel when their file scopes permit, and integrates and validates the final Project Pulse experience.

Inspect .github/agents/ and summarize the custom agent team I will use to build
Mona's Project Pulse dashboard.

Update the replace text in `docs/agent-team.md`.