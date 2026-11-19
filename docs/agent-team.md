# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team that builds Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Agent definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 5.5 (copilot) | Coordinates the Planner, Coder, and Designer; breaks the request into phases, assigns file scopes, delegates work, and verifies the integrated result. | [`.github/agents/orchestrator.agent.md`](../.github/agents/orchestrator.agent.md) |
| Planner | Claude Opus 5.5 (copilot) | Researches the repository and requirements, identifies dependencies and edge cases, and produces an ordered implementation plan. | [`.github/agents/planner.agent.md`](../.github/agents/planner.agent.md) |
| Coder | GPT-5.5 (copilot) | Implements code and runnable-app support within its assigned file scope, using clear, testable patterns and validating the result. | [`.github/agents/coder.agent.md`](../.github/agents/coder.agent.md) |
| Designer | Gemini 3.7 Flash (copilot) | Designs the dashboard UI/UX, accessibility, responsive layout, visual hierarchy, interaction flow, and Project Pulse styling. | [`.github/agents/designer.agent.md`](../.github/agents/designer.agent.md) |

## How the team builds Project Pulse

The Orchestrator starts by asking the Planner to research the existing repository and create an implementation plan for Project Pulse. The Planner identifies file assignments, dependencies, risks, edge cases, and validation expectations.

The Orchestrator converts that plan into coordinated phases and gives the Coder and Designer explicit, non-overlapping file scopes when their work can run in parallel. The Designer establishes the dashboard's polished visual direction and responsive UI patterns, including project cards, status badges, priority treatment, and accessible interactions. The Coder implements the application structure and behavior, using deterministic hooks and configuration needed to run the dashboard.

The team executes dependent work in sequence, with the Orchestrator coordinating handoffs and verifying that the integrated frontend hangs together. It presents the final dashboard, documents the files changed, and reports validation results and any remaining risks.
