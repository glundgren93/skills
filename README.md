# Skills

Reusable [Agent Skills](https://agentskills.io/) for Pi and compatible coding agents.

## Included skills

- [`be-brief`](skills/be-brief/SKILL.md) — make responses concise and actionable.
- [`read-later`](skills/read-later/SKILL.md) — park distracting ideas in an Obsidian vault and gently resume one later.
- [`test-signal-audit`](skills/test-signal-audit/SKILL.md) — audit and improve low-signal tests using mutation evidence.

## Install for Pi

```bash
pi install https://github.com/glundgren93/skills
```

This installs the skills and prompt templates. Use `pi config` to enable or disable individual resources.

## Read Later

The package adds two low-friction commands:

```text
/read-later <idea, link, question, or side quest>
/read-now [title, number, tag, or search phrase]
```

`/read-later` captures the distraction without researching it, records the current context, and gives it a small re-entry plan. `/read-now` selects one parked item and offers a tiny first step instead of dropping the user into the whole topic at once.

On first use, the agent asks for the absolute path to the Obsidian vault and writes it into the configuration block of the locally installed `SKILL.md`. The default vault folder is `Read Later`; asking the agent to change vault updates that same local block.

Compatible agents without Pi prompt templates can invoke `/skill:read-later` and ask for **Capture mode** or **Resume mode**.
