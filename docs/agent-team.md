# Agent team for Mona's Project Pulse dashboard

We will use a four-agent custom team to build Mona's Project Pulse dashboard, with the work coordinated through GitHub Copilot CLI in a Codespace.

- Orchestrator — Model: Claude Opus 5.5 (copilot). Responsibilities: breaks the work into phases, delegates to specialist agents, manages dependencies, and verifies the combined result. Definition: `.github/agents/orchestrator.agent.md`.
- Planner — Model: Claude Opus 5.5 (copilot). Responsibilities: researches the repo and requirements, identifies edge cases, and produces an implementation plan with file assignments, sequencing, and validation expectations. Definition: `.github/agents/planner.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibilities: implements the logic, fixes bugs, and builds runnable app support within the file scope assigned by the Orchestrator. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.7 Flash (copilot). Responsibilities: handles dashboard UX, accessibility, information hierarchy, and styling for the Project Pulse front-end. Definition: `.github/agents/designer.agent.md`.

Together, these custom agents live under the repository's agent folder (`.github/agents/`) and give us a clear division of labor: planning, design, implementation, and orchestration for the dashboard work.
