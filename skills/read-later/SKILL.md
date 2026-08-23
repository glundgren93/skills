---
name: read-later
description: Park distracting ideas, links, questions, and side quests in a configured Obsidian vault, then reintroduce one gently when the user is ready. Use when the user says "read later", "save this for later", "park this", "don't let me get sidetracked", "read now", "resume a distraction", or asks to browse their parked topics.
---

# Read Later

Protect the user's current focus. Capture a distraction with enough context to make it easy to resume, but **do not investigate the distraction while capturing it**.

This skill has two modes:

- **Capture**: park one distraction, then point the user back to their current task.
- **Resume**: find one parked item and provide a small re-entry ramp.

When invoked as `/skill:read-later`, treat arguments beginning with `resume` or `read-now` as Resume mode. Treat every other argument as Capture mode.

## Local configuration

This block belongs to the installed local copy of the skill. On first use, replace `UNCONFIGURED` with the user's chosen absolute vault path.

```yaml
vault_path: UNCONFIGURED
folder: Read Later
```

If `vault_path` is `UNCONFIGURED`:

1. Keep the pending distraction or resume request in conversation context.
2. Ask only: **“What is the absolute path to the Obsidian vault you want to use?”**
3. Verify that the directory exists. Prefer a directory containing `.obsidian`; if it does not, explain that it may not be an Obsidian vault and ask for confirmation.
4. Edit the configuration block in this `SKILL.md` file with the absolute path.
5. Create the configured folder and index when needed, then continue the original request.

Do not scan the filesystem for vaults or silently choose one. If the user asks to change vault, update this block. The folder must be relative to the vault and must not contain `..`.

## Vault layout

```text
<configured vault>/
└── <configured folder>/
    ├── Read Later.md
    └── YYYY-MM-DD HHmm - descriptive-slug.md
```

Operate only inside the configured folder. Never read unrelated vault notes.

Use local time and a short lowercase kebab-case slug for filenames. If a filename exists, add `-2`, `-3`, and so on. Never overwrite a note.

## Non-negotiable behavior

- During capture, do not browse, research, explain, solve, or expand the parked topic.
- Preserve the user's wording and any URLs or file references they supplied.
- Add only context already present in the conversation or current workspace.
- Keep responses short. The system exists to reduce attention switching, not create another workflow.
- Ask exactly one question when blocked.

## Capture mode

Treat the command argument as the distraction. If it is empty and the distraction cannot be inferred, ask only: **“What should I park for later?”**

Create one note with this structure:

```markdown
---
type: read-later
title: "Human-readable title"
status: unread
captured: 2026-08-23T10:15:00+02:00
last_opened: null
completed: null
source_project: "project name or null"
source_path: "/absolute/project/path or null"
tags:
  - read-later
  - topic/example
aliases: []
---

# Human-readable title

## The distraction

The user's original wording, link, or question.

## Why I saved it

One or two factual sentences based only on available context.

## Where it came up

- **While doing:** the active task, or `Unknown`
- **Return to now:** the focus to resume after capture, or `Unknown`
- **Conversation context:** a compact summary that will still make sense later
- **Anchors:** supplied URLs, Obsidian links, or `path:line` references; `None supplied` if absent

## Re-entry ramp

1. **Orient (2 min):** what to reread or recall first.
2. **First action:** one concrete action, not a whole project.
3. **Timebox:** 10–25 minutes unless the user supplied another limit.
4. **Stop when:** a visible stopping condition.

## Open questions

- Unverified questions worth answering when resumed. Do not answer them during capture.

## Session log

- 2026-08-23T10:15:00+02:00 — Captured.
```

Infer a useful title and at most three broad `topic/` tags. Include the raw distraction, why it caught the user's attention without inventing motives, where it surfaced, what they should return to now, and a small re-entry plan.

Use `unread`, `active`, or `done` for status. Keep `read-later` as the first tag.

Update `Read Later.md` immediately and put the new item first in **Inbox**.

Reply with no more than three short lines:

1. `Parked: <title>`
2. the relative vault path;
3. `Back to: <the task they were doing>` when known.

Do not summarize the distracting topic in the reply.

## Index

Create the index if missing:

```markdown
---
type: read-later-index
tags:
  - read-later/index
---

# Read Later

A low-friction shelf for ideas that can wait. Newest items appear first.

## Inbox

- [ ] [[2026-08-23 1015 - descriptive-slug|Human-readable title]] — why it was saved · 2026-08-23

## Active

## Done

```

Every note appears exactly once. `unread` belongs in Inbox, `active` in Active, and `done` in Done. Done items use checked boxes; other items use unchecked boxes. Keep newest items first and use vault-relative Obsidian links without `.md`.

## Resume mode

### Select one item

If the user supplied a title, phrase, number from a just-shown list, or Obsidian link, search only the configured folder. Match in this order:

1. exact note title or filename;
2. exact alias;
3. case-insensitive title substring;
4. tags and note text.

If one result is clearly best, use it. If results are ambiguous, show at most five numbered matches and ask the user to choose one.

If no selector was supplied, read the index and show at most five Inbox/Active items, newest first. Give each one a short “why saved” phrase and ask the user to choose. Do not introduce several topics at once.

### Reintroduce gently

For the selected note:

1. Read that note only.
2. Update `last_opened` with the current local timestamp. Do not change status yet.
3. Present:
   - **Saved because:** one sentence;
   - **You were doing:** one sentence;
   - **Tiny first step:** one action fitting in 2–10 minutes;
   - **Stop after:** the stored stopping point or timebox.
4. Ask one question: **Start now, keep parked, or mark done?**

Do not research or execute the topic until the user chooses **Start now**.

- **Start now:** set status to `active`, move the index link to **Active**, help with only the tiny first step, and append a session-log entry.
- **Keep parked:** leave status and index unchanged. End without guilt or persuasion.
- **Mark done:** set status to `done`, set `completed`, move the index link to **Done**, and keep the note.

## Repair behavior

If the index is missing or inconsistent, rebuild it from notes inside the configured folder only. Preserve every note and never infer `done` merely from age.
