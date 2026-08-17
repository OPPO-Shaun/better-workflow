# better-workflow

A repository configured with [Matt Pocock's agent skills](https://github.com/mattpocock/skills) for GitHub Copilot-compatible project use.

## Agent Skills

All skills are installed under `.github/skills/` and come from [mattpocock/skills](https://github.com/mattpocock/skills).

Categories installed:

- **[Engineering](.github/skills/engineering/README.md)** — code review, TDD, domain modeling, bug diagnosis, architecture, and more
- **[Productivity](.github/skills/productivity/README.md)** — grilling, teaching, handoff, questionnaires, and more
- **[Misc](.github/skills/misc/README.md)** — git guardrails, pre-commit hooks, exercise scaffolding, and more

See [SKILLS_SOURCE.md](./SKILLS_SOURCE.md) for:
- The pinned upstream commit SHA
- Full skill listing with descriptions
- How to update/resync skills
- How to run the one-time `/setup-matt-pocock-skills` flow
- Limitations per tool (Copilot Cloud Agent, CLI, VS Code, Claude Code, SDK)

## Agent Files

Agent definition files from upstream are installed under `.github/agents/`:

- `install-block.md` — guidelines for the installation block pattern
- `invocation.md` — guidelines for skill invocation
- `writing-docs.md` — guidelines for writing agent documentation