# Skills

Reusable [Agent Skills](https://agentskills.io/) for Pi and compatible coding agents.

## Included skills

- [`be-brief`](skills/be-brief/SKILL.md) — make responses concise and actionable.
- [`test-signal-audit`](skills/test-signal-audit/SKILL.md) — audit and improve low-signal tests using mutation evidence.

## Install for Pi

```bash
pi install https://github.com/glundgren93/skills
```

This installs both skills and exposes them as `/skill:be-brief` and
`/skill:test-signal-audit`. Use `pi config` to enable or disable individual
skills.
