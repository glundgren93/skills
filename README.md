# Skills

Reusable [Agent Skills](https://agentskills.io/) for Pi and compatible coding agents.

## Included skills

- [`be-brief`](skills/be-brief/SKILL.md) — make responses concise and actionable.
- [`for-later`](skills/for-later/SKILL.md) — save distracting topics with enough origin context for a future conversation or agent.
- [`for-now`](skills/for-now/SKILL.md) — choose a saved topic, restore its origin, and decide how to continue.
- [`test-signal-audit`](skills/test-signal-audit/SKILL.md) — audit and improve low-signal tests using mutation evidence.

## Install for Pi

```bash
pi install https://github.com/glundgren93/skills
```

This installs the skills. Use `pi config` to enable or disable individual resources.

## For Later / For Now

Save a distraction without exploring it:

```text
/skill:for-later <idea, link, question, or side quest>
```

Each note records where the topic came from, relevant context and anchors, open questions, and a handoff for a future agent.

Deliberately return to something you saved:

```text
/skill:for-now
```

`for-now` shows every saved topic with its origin. After the user chooses one, it restores the context and asks what they want to do before acting.

On first use, the agent asks for the absolute path to the Obsidian vault and writes it into the configuration block of the locally installed `for-later` skill. Both skills use that configuration. The default vault folder is `For Later`.
