# Skills

Reusable [Agent Skills](https://agentskills.io/) for Pi and compatible coding agents.

## Included skills

- [`be-brief`](skills/be-brief/SKILL.md) — make responses concise and actionable.
- [`test-signal-audit`](skills/test-signal-audit/SKILL.md) — audit and improve low-signal tests using mutation evidence.

## Install for Pi

Clone the repository, then symlink the skills you want into Pi's global skill directory:

```bash
git clone https://github.com/glundgren93/skills.git agent-skills
mkdir -p ~/.pi/agent/skills
ln -s "$PWD/agent-skills/skills/be-brief" ~/.pi/agent/skills/be-brief
ln -s "$PWD/agent-skills/skills/test-signal-audit" ~/.pi/agent/skills/test-signal-audit
```

Pi discovers directories containing `SKILL.md` and exposes them as `/skill:<name>` commands.
