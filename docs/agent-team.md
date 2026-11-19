# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate a four-agent team that plans, designs, builds, and validates Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Agent definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 5.5 (copilot) | Turns the request into ordered phases, delegates to the specialist agents with explicit file scopes, and verifies the integrated result. | [`.github/agents/orchestrator.agent.md`](../.github/agents/orchestrator.agent.md) |
| Planner | Claude Opus 5.5 (copilot) | Researches the repository and requirements, identifies dependencies and edge cases, and delivers an ordered plan with file assignments and validation expectations. | [`.github/agents/planner.agent.md`](../.github/agents/planner.agent.md) |
| Coder | GPT-5.5 (copilot) | Implements assigned application code and runnable-app support using clear, deterministic, testable patterns, then validates the result. | [`.github/agents/coder.agent.md`](../.github/agents/coder.agent.md) |
| Designer | Gemini 3.7 Flash (copilot) | Leads UI/UX, accessibility, responsive layout, visual hierarchy, interaction flow, and Project Pulse styling within an assigned design scope. | [`.github/agents/designer.agent.md`](../.github/agents/designer.agent.md) |

## How the team builds Project Pulse

The Orchestrator asks the Planner to research the existing repository and create an implementation plan for Project Pulse. The Planner identifies file assignments, dependencies, risks, edge cases, and validation expectations.

The Orchestrator converts that plan into phases and delegates work to the Designer and Coder only with non-overlapping file scopes when their tasks can run in parallel. It sequences overlapping or dependent work instead. The Designer establishes the polished visual direction and responsive UI patterns, including project cards, status badges, priority treatment, and accessible interactions. The Coder implements the application structure and behavior and, when assigned, creates the launch configuration needed to run the dashboard.

The team executes dependent work in sequence, with the Orchestrator coordinating handoffs and verifying that the integrated frontend hangs together. It presents the final dashboard, documents the files changed, and reports validation results and any remaining risks.
