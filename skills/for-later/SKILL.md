---
name: for-later
description: Save distracting ideas, links, questions, and side quests as agent-ready context in an Obsidian vault, then restore where a topic came from before following up. Use when the user says "for later", "read later", "save this for later", "park this", "don't let me get sidetracked", "follow up on", or asks to revisit a saved topic.
---

# For Later

Save enough context that the user—or a fresh agent—can later understand where a distracting topic came from without reconstructing the original conversation.

This skill has two modes:

- **Capture**: save one distraction without exploring it. This is the default.
- **Follow up**: restore one saved topic, then ask what direction the user wants before acting.

When invoked as `/skill:for-later`, treat arguments beginning with `follow-up`, `follow up`, `resume`, `revisit`, or `read-now` as Follow-up mode. Treat every other argument as Capture mode.

## Local configuration

This block belongs to the installed local copy of the skill. On first use, replace `UNCONFIGURED` with the user's chosen absolute vault path.

```yaml
vault_path: UNCONFIGURED
folder: For Later
```

If `vault_path` is `UNCONFIGURED`:

1. Keep the pending topic or follow-up request in conversation context.
2. Ask only: **“What is the absolute path to the Obsidian vault you want to use?”**
3. Verify that the directory exists. Prefer a directory containing `.obsidian`; if it does not, explain that it may not be an Obsidian vault and ask for confirmation.
4. Edit the configuration block in this local `SKILL.md`.
5. Create the configured folder and index if needed, then continue the original request.

Do not scan for vaults or silently choose one. If the user asks to change vault, update this block. The folder must be relative to the vault and must not contain `..`.

## Storage

```text
<configured vault>/
└── <configured folder>/
    ├── For Later.md
    └── YYYY-MM-DD HHmm - descriptive-slug.md
```

For compatibility, if `Read Later.md` already exists in the configured folder and `For Later.md` does not, use the existing file as the index. Do not rename or move existing vault content without asking.

Operate only inside the configured folder. Never read unrelated vault notes. Use local time and a short lowercase kebab-case slug for filenames. On collision, add `-2`, `-3`, and so on; never overwrite a note.

## Capture

Treat the command argument as the topic. If it is empty and cannot be inferred, ask only: **“What should I save for later?”**

Do not browse, research, solve, explain, or expand the topic. Use only the user's wording and context already available in the conversation or current workspace.

Create one note:

```markdown
---
type: for-later
title: "Human-readable title"
captured: 2026-08-23T10:15:00+02:00
source_project: "project name or null"
source_path: "/absolute/project/path or null"
tags:
  - for-later
  - topic/example
aliases: []
---

# Human-readable title

## Topic

The user's original wording, link, or question.

## Where this came from

- **While doing:** the task or conversation in progress
- **Why it surfaced:** the connection that caused this topic to appear
- **Why it was saved:** why following it immediately would leave the current focus
- **Source:** project, working directory, conversation, or `Unknown`

## Context already established

A compact, factual account of what was already discussed or known. Include relevant decisions, constraints, terminology, and assumptions. Do not add newly researched information.

## Anchors

- URLs supplied by the user
- Obsidian links
- Relevant `path:line` references
- Commit, issue, or document identifiers
- `None supplied` when there are no anchors

## Questions and possible threads

- Unanswered questions or directions that made the topic interesting

## Handoff for an agent

First restore this topic's origin and summarize the useful context. Then ask the user what direction they want to take. Do not begin research, implementation, or file changes until they answer.
```

Infer a useful title and at most three broad `topic/` tags. Preserve uncertainty: label guesses or incomplete context instead of inventing details.

Update the index immediately. Add the new item first in the single list:

```markdown
---
type: for-later-index
tags:
  - for-later/index
---

# For Later

Topics saved with enough origin context for a future conversation or agent.

- [[2026-08-23 1015 - descriptive-slug|Human-readable title]] — came up while <short origin> · 2026-08-23
```

There are no inbox, active, done, completion, or checkbox states. Every saved note appears exactly once, newest first. Use vault-relative Obsidian links without `.md`.

Reply with no more than three short lines:

1. `Saved for later: <title>`
2. the relative vault path;
3. `Back to: <the task they were doing>` when known.

Do not summarize or discuss the distracting topic in the capture reply.

## Follow up

### Select one topic

If the user supplied a title, phrase, number from a just-shown list, or Obsidian link, search only the configured folder. Prefer exact title or filename, then aliases, title substrings, tags, and finally note text.

If one result is clearly best, use it. If ambiguous, show at most five numbered matches with one short origin phrase and ask the user to choose.

If no selector was supplied, read the index and show at most seven newest topics. Give each one a short origin phrase and ask the user to choose one. Do not introduce several topics at once.

### Restore context, then stop

Read the selected note only. Present a compact context restoration:

- **Topic:** the saved question or idea;
- **Where it came from:** what the user was doing and why it surfaced;
- **Already established:** the useful facts, constraints, and decisions;
- **Anchors:** the most relevant links or file references;
- **Open threads:** the saved unanswered directions.

Then ask exactly one question: **“What direction do you want to take this?”**

Do not research, execute commands for the topic, modify project files, or choose a direction until the user answers. After they answer, use the saved note as context and follow their direction normally.

Do not add lifecycle state or update the note merely because it was opened. Only change a saved note when the user explicitly asks to add, correct, or remove context.

## Repair

If the index is missing or inconsistent, rebuild the single newest-first list from notes inside the configured folder. Treat legacy `type: read-later` notes as saved topics, ignore their old status fields, and preserve their content. Never delete or rewrite notes during an index repair.
