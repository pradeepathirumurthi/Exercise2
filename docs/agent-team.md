# Agent team

GitHub Copilot CLI in a Codespace will orchestrate the custom agent team building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| **Orchestrator** | Claude Opus 5.5 (copilot) | Breaks requests into phases, delegates scoped work to specialists, coordinates dependencies, verifies integration, and reports outcomes. It coordinates rather than implementing. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 5.5 (copilot) | Researches the codebase, documentation, dependencies, and edge cases, then provides an ordered implementation plan with file ownership, dependencies, risks, and validation expectations. | `.github/agents/planner.agent.md` |
| **Designer** | Gemini 3.7 Flash (copilot) | Shapes the dashboard's user experience, accessibility, information hierarchy, interactions, visual design, and responsive behavior. For Project Pulse, it guides polished project cards, status badges, priority treatment, and clear layout. | `.github/agents/designer.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements application logic and assigned code, fixes bugs, and validates changes. For runnable Project Pulse work, it can configure a deterministic launch setup that opens the dashboard. | `.github/agents/coder.agent.md` |
