# Mentor Me Templates

Use these as compact shapes, not mandatory prose. Keep the live conversation shorter than the template when the learner needs less context.

## Primer

```markdown
### Theory primer: <concept>

**Problem:** <what this solves>

**Data flow:**
`<input> -> <component> -> <next component>`

**Minimum vocabulary:**
- `<term>` — <plain-English meaning>

**Worked example:**
1. <small concrete step>
2. <small concrete step>
3. <observable result>

**Invariants:**
- <invariant> — if violated, <failure>

Optional reading: `<local path or documentation>`
```

## Decision question

```markdown
### **Required behavior — <plain-English title>**

<Small concrete case.>

<One focused prediction or explanation question.>
```

or:

```markdown
### **Your choice — <plain-English title>**

<Small concrete case.>

1. <option and tradeoff>
2. <option and tradeoff>

<One focused choice question.>
```

## Decision ledger

```markdown
| Decision | Learner's conclusion | Invariant/evidence | Consequence |
|---|---|---|---|
| <decision> | <answer or delegated choice> | <why> | <code-visible effect> |
```

## Slice authorization

```markdown
## Proposed slice: <name>

**Accepted decisions**
- <decision and consequence>

**Expected changes**
- `<path>` — <behavior>

**Outside this slice**
- <deferred behavior>

**Validation**
- `<command>`

Do you authorize this slice?
```

## Slice review

```markdown
## Slice completed

**Changed**
- `<path>` — <behavior>

**Validation**
- `<command>` — <outcome>

**Boundary not yet established**
- <case>

### Review this important line

`<path:line>`

```<language>
<line or small comparison>
```

1. What is this line trying to do?
2. What plausible behavior would break if it changed?
```

## Learning journal

Create project-root `LEARNING_JOURNAL.md` with this header:

```markdown
# Learning Journal

A brief, append-only record of concepts demonstrated while learning this codebase.
```

Append entries chronologically:

```markdown
## YYYY-MM-DD — <slice name>

**Learned**
- <two to four concise concepts demonstrated by the learner>

**Code connection**
- `<path or symbol>` — <how the concept appears in code>

**Evidence**
- `<command>` — <outcome>

**Still unresolved**
- <next concept or `None for this slice`>
```

Keep an entry roughly 5–12 lines. Do not turn it into a command transcript or duplicate the full decision ledger.

## Pause or checkpoint handoff

```markdown
## Handoff

**Decisions:** <compact ledger or link>

**Changed:** <paths and behavior>

**Invariants:** <key correctness claims>

**Validated:** <commands and outcomes>

**Boundary:** <unproven case>

**Resume with:** <one unresolved question>
```
