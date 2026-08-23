# Skills

Reusable [Agent Skills](https://agentskills.io/) for Pi and compatible coding agents.

## Included skills

- [`be-brief`](skills/be-brief/SKILL.md) — make responses concise and actionable.
- [`for-later`](skills/for-later/SKILL.md) — save distracting topics with enough origin context for a future conversation or agent.
- [`test-signal-audit`](skills/test-signal-audit/SKILL.md) — audit and improve low-signal tests using mutation evidence.

## Install for Pi

```bash
pi install https://github.com/glundgren93/skills
```

This installs the skills. Use `pi config` to enable or disable individual resources.

## For Later

```text
/skill:for-later <idea, link, question, or side quest>
/skill:for-later follow-up [title, number, tag, or search phrase]
```

The default mode saves a distraction without exploring it. Each note records where the topic came from, relevant context and anchors, open questions, and a handoff for a future agent. `follow-up` restores one topic and asks what direction the user wants before acting.

On first use, the agent asks for the absolute path to the Obsidian vault and writes it into the configuration block of the locally installed `SKILL.md`. The default vault folder is `For Later`; asking the agent to change vault updates that same local block.
