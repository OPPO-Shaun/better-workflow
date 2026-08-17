# Skills Source

The skills in `.github/skills/` are copied from [Matt Pocock's `mattpocock/skills`](https://github.com/mattpocock/skills) repository.

## Pinned Revision

| Field | Value |
|---|---|
| Upstream repo | `https://github.com/mattpocock/skills` |
| Commit SHA | `068b6e0c62393147daf03530149cdce209c93da8` |
| Snapshot date | 2026-08-17 |

## Installed Skills

### Engineering (`skills/engineering/`)

| Skill | Description |
|---|---|
| `ask-matt` | Ask questions to the agent as if it were Matt Pocock |
| `code-review` | Systematic code review |
| `codebase-design` | Design a codebase architecture |
| `diagnosing-bugs` | Diagnose and fix bugs |
| `domain-modeling` | Model the domain of a codebase |
| `grill-with-docs` | Interrogate documentation to answer questions |
| `implement` | Implement a feature |
| `improve-codebase-architecture` | Improve the architecture of a codebase |
| `prototype` | Build a prototype |
| `research` | Research a topic |
| `resolving-merge-conflicts` | Resolve merge conflicts |
| `setup-matt-pocock-skills` | One-time setup flow for Matt Pocock's skills |
| `tdd` | Test-driven development |
| `to-spec` | Write a specification |
| `to-tickets` | Break a spec into actionable tickets |
| `triage` | Triage issues |
| `wayfinder` | Navigate a codebase |
| `wizard` | Build something complex step-by-step |

### Productivity (`skills/productivity/`)

| Skill | Description |
|---|---|
| `grill-me` | Be relentlessly interviewed about a plan or design |
| `grilling` | Interview the user about a plan, decision, or idea |
| `handoff` | Compact a conversation into a handoff document |
| `teach` | Teach the user a skill over multiple sessions |
| `to-questionnaire` | Turn a decision into a Markdown questionnaire |
| `wait-what` | Re-pitch a message that didn't land |
| `writing-for-agents` | Writing documents for agents |

### Misc (`skills/misc/`)

| Skill | Description |
|---|---|
| `git-guardrails-claude-code` | Set up Claude Code hooks to block dangerous git commands |
| `migrate-to-shoehorn` | Migrate `as` type assertions to `@total-typescript/shoehorn` |
| `scaffold-exercises` | Create exercise directory structures |
| `setup-pre-commit` | Set up Husky pre-commit hooks |

## How to Update / Resync

To update the skills to a new upstream commit:

1. Find the desired commit SHA from https://github.com/mattpocock/skills/commits/main
2. Run the following commands (replace `<NEW_SHA>` with the actual commit SHA):

```bash
SHA="<NEW_SHA>"
BASE="https://raw.githubusercontent.com/mattpocock/skills/${SHA}"

# Re-fetch all engineering skills
for s in ask-matt code-review codebase-design diagnosing-bugs domain-modeling grill-with-docs implement improve-codebase-architecture prototype research resolving-merge-conflicts setup-matt-pocock-skills tdd to-spec to-tickets triage wayfinder wizard; do
  curl -sf "${BASE}/skills/engineering/${s}/SKILL.md" -o ".github/skills/engineering/${s}/SKILL.md"
done

# Re-fetch all productivity skills
for s in grill-me grilling handoff teach to-questionnaire wait-what writing-for-agents; do
  curl -sf "${BASE}/skills/productivity/${s}/SKILL.md" -o ".github/skills/productivity/${s}/SKILL.md"
done

# Re-fetch all misc skills
for s in git-guardrails-claude-code migrate-to-shoehorn scaffold-exercises setup-pre-commit; do
  curl -sf "${BASE}/skills/misc/${s}/SKILL.md" -o ".github/skills/misc/${s}/SKILL.md"
done

# Re-fetch category READMEs and agents
curl -sf "${BASE}/skills/engineering/README.md" -o ".github/skills/engineering/README.md"
curl -sf "${BASE}/skills/productivity/README.md" -o ".github/skills/productivity/README.md"
curl -sf "${BASE}/skills/misc/README.md" -o ".github/skills/misc/README.md"
```

3. Update the pinned revision table in this file.
4. Commit the changes with an appropriate commit message.

## Running the One-Time Setup Flow

The `setup-matt-pocock-skills` skill provides a one-time setup flow. To invoke it:

- **GitHub Copilot CLI / VS Code / Claude Code**: Run `/setup-matt-pocock-skills` in an agent session
- **GitHub Copilot Cloud Agent**: Mention the skill in your prompt: *"Run the setup-matt-pocock-skills skill to configure this repository"*

The setup flow will walk through configuring skills, issue trackers, and other project-level settings.

## Limitations

| Tool | Notes |
|---|---|
| **GitHub Copilot Cloud Agent** | Skills in `.github/skills/` are available if the agent reads the repo context. The `/setup-matt-pocock-skills` slash command may not be directly supported; invoke it by describing the task in your prompt. |
| **GitHub Copilot CLI / VS Code** | Full skill invocation via `/skill-name` slash commands is supported when using the Copilot CLI or VS Code extension. |
| **Claude Code** | Claude Code reads `CLAUDE.md` and can use skills via the `@skill-name` invocation pattern. |
| **Copilot SDK** | Pass `.github/skills` as a `skillDirectories` entry when creating a session. |
