# Agent team

For Mona's Project Pulse dashboard, I will use this custom agent team, orchestrated with GitHub Copilot CLI in a Codespace.

- Orchestrator — Model: Claude Opus 5.5 (copilot). Responsibility: coordinates the overall project by breaking the dashboard into phases, delegating work to specialist agents, and verifying that the final result fits together cleanly. Definition: .github/agents/orchestrator.agent.md.
- Planner — Model: Claude Opus 5.5 (copilot). Responsibility: researches the repo and requirements, identifies dependencies and edge cases, and produces the implementation plan with file ownership, validation expectations, and sequencing. Definition: .github/agents/planner.agent.md.
- Designer — Model: Gemini 3.7 Flash (copilot). Responsibility: focuses on UX, accessibility, visual polish, hierarchy, and responsive dashboard styling for the Project Pulse experience. Definition: .github/agents/designer.agent.md.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implements code changes and logic in the assigned file scope, including runnable app setup when needed for the dashboard preview. Definition: .github/agents/coder.agent.md.

These agents live under the repository's custom agent folder, .github/agents/, and work together to plan, design, and implement Mona's Project Pulse dashboard.
