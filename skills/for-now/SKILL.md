---
name: for-now
description: Revisit topics saved by the for-later skill. Show every saved topic with where it came from, let the user choose one, restore its context, and ask what they want to do before acting. Use when the user says "for now", "show saved topics", "what did I save", "continue a saved topic", "follow up on something", or wants to return to a parked distraction.
---

# For Now

Help the user deliberately return to one topic saved by the companion `for-later` skill.

The interaction has two stops:

1. Show every saved topic and ask the user to choose one.
2. Restore the selected topic's origin and ask what they want to do with it.

Do not research, implement, browse for the topic, or modify project files before the user gives a direction.

## Find the configured vault

Read the **Local configuration** block from the sibling `../for-later/SKILL.md`. That file is the single source of truth for `vault_path` and `folder`; never duplicate configuration in this skill.

If the sibling skill is missing, explain that `for-now` requires `for-later` and stop.

If `vault_path` is `UNCONFIGURED`:

1. Ask only: **“What is the absolute path to the Obsidian vault you want to use?”**
2. Verify that the directory exists. Prefer a directory containing `.obsidian`; if it does not, explain that it may not be an Obsidian vault and ask for confirmation.
3. Edit the configuration block in the local `../for-later/SKILL.md`.
4. Create the configured folder and index if needed.
5. Continue listing topics.

The folder must remain inside the vault. Operate only inside that configured folder and never read unrelated vault notes.

Use `<folder>/For Later.md` as the index. For compatibility, if `Read Later.md` exists and `For Later.md` does not, use `Read Later.md`.

## First stop: show every topic

On initial invocation, always show every saved topic. Do not ask for a search phrase and do not silently select one.

Read the index and extract every Obsidian note link, including links under legacy Inbox, Active, or Done headings. Ignore checkboxes and lifecycle labels.

Present a numbered list, newest first:

```text
1. Topic title — where it came from · saved date
2. Another topic — where it came from · saved date
```

Keep each item to one line. Show all topics, not only a recent subset. End with exactly one question:

**“Which topic do you want to continue?”**

If there are no topics, say so and suggest `/skill:for-later <topic>`; do not invent entries.

If the index is missing or does not contain all saved notes, rebuild a single newest-first list from `type: for-later` and legacy `type: read-later` notes in the configured folder. Preserve every note and never rewrite note contents during repair.

## Resolve the selection

Accept a number from the list, title, filename, Obsidian link, or unmistakable phrase. Match exact title or filename first, then aliases and title substrings.

If the selection is ambiguous, show only the matching choices and ask again. If no note matches, say that and show the list again.

Read only the selected note.

## Second stop: restore context

Present a compact restoration:

- **Topic:** the saved idea or question;
- **Where it came from:** what the user was doing and why it surfaced;
- **Already established:** useful facts, constraints, decisions, and terminology;
- **Anchors:** relevant links, files, commits, issues, or documents;
- **Open threads:** saved unanswered questions or possible directions.

For legacy notes, derive these fields from sections such as **The distraction**, **Why I saved it**, **Where it came up**, **Open questions**, and **Re-entry ramp**. Ignore status, completion, and session-log fields.

If the user's selection message did not include a clear direction, end with exactly:

**“What would you like to do with this?”**

Then stop. Do not choose a direction for them.

If the selection message already included a clear direction, restore the context briefly and follow that direction without asking again.

## Continue

After the user gives a direction, use the saved note as context and help normally. Research, commands, and file changes are allowed only as required by that direction and the current workspace instructions.

Do not add status, completion, or last-opened metadata. Do not modify the saved note merely because it was selected. Only update or remove saved context when the user explicitly asks.
