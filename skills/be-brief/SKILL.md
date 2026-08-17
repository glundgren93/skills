---
name: be-brief
description: Cut a reply down to what the user can act on. Use when the user says "be brief", "too long", "wall of text", "TL;DR", "get to the point", "I have no idea what you want", "shorter", or reacts to a long answer with confusion rather than an answer to it. Also apply proactively when about to present a plan, a review, or a survey result.
---

# Be Brief

The failure this fixes: a reply so long the user can't find the decision inside it. They then answer nothing, and the work stalls. Length is not thoroughness — an unanswerable message is a failed message.

## The rule

**One question, or one action. Not both, not many.**

If you need something from the user, ask exactly one thing and stop. If you don't, do the work and report in a few lines.

## When the user pushes back on length

They are not asking you to summarize what you wrote. They're saying it didn't land. So:

1. Do NOT re-post a condensed version of the same content. That's another wall.
2. Identify the single blocking decision.
3. Ask it with `AskUserQuestion`, 2-3 options, short labels.
4. Say nothing else.

Two sentences of lead-in, maximum. Usually zero.

## Structure that works

- No headers for anything under ~15 lines.
- No tables unless comparing 3+ things on 2+ axes.
- No "Unresolved questions" list — that's how you end up asking five things and getting none answered. Pick the one that blocks you.
- No recap of what you just did. They were there.
- No restating the user's request back to them.

## Plans

A plan the user can't read is not approved, it's ignored. Numbered steps with file paths, one line each. Cut every step that's obvious (`run the tests`, `commit`). If the plan needs more than ~10 lines, the scope is too big to approve in one go — propose the first slice instead.

## What survives the cut

Keep: the decision, the tradeoff, the thing that will surprise them, the file:line.
Cut: reasoning they didn't ask for, alternatives you rejected, caveats that don't change what happens next, praise for their question, and anything hedging what you already verified.

If a caveat genuinely matters, it's one sentence, not a section.

## Asking well

Use `AskUserQuestion` rather than prose questions — it's clickable, so the cost of answering drops to one keystroke. Put your recommendation first and mark it. Options are 1-5 words; the tradeoff goes in the description, not the label.

One question per turn. If you think you need four, three of them are things you should decide yourself and mention in one line afterward.

## Self-check before sending

- Is there exactly one thing I want from them, and is it obvious what it is?
- Would deleting my first paragraph lose anything?
- Am I explaining something they already know?
- Any section here purely to show I did the work?

If a draft has multiple headers and a question at the bottom, the question won't be seen. Lead with it.
