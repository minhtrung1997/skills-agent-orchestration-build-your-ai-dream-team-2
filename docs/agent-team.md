# Agent team

For Mona's Project Pulse dashboard, I will use a four-agent custom team orchestrated through GitHub Copilot CLI inside a Codespace. The agent definitions live in the repository's `.github/agents/` folder and are assigned to specialized roles so the work can be planned, designed, built, and coordinated without overlap.

- Planner — Model: Claude Opus 4.7 (copilot). Responsibility: research the repository, inspect docs and dependencies, identify edge cases and validation needs, and produce an implementation plan with phases, file ownership, dependencies, and risks. Definition: `.github/agents/planner.agent.md`.
- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsibility: break the plan into workstreams, delegate tasks to the specialist agents, preserve file-scope boundaries, sequence parallel work when safe, and verify the integrated result. Definition: `.github/agents/orchestrator.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsibility: shape the Project Pulse user experience, accessibility, visual hierarchy, responsive layout, and dashboard styling so the first view reads clearly as a polished project status dashboard. Definition: `.github/agents/designer.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implement the app logic and UI code in the assigned file scope, fix bugs, and add runnable app support when needed for the Project Pulse experience. Definition: `.github/agents/coder.agent.md`.

This workflow uses GitHub Copilot CLI in a Codespace as the orchestration layer, with the planner and orchestrator driving delegation and the designer and coder handling specialized implementation work.