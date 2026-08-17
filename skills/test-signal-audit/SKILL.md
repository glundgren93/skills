---
name: test-signal-audit
description: Audit, prove, remove, or rewrite low-signal automated tests using mutation evidence. Use when the user asks to audit test quality, cull or shrink a test suite, identify useless/brittle/shape/typecheck-only tests, prove tests are useless, delete low-value tests, or open a test-cleanup PR.
---

# Test Signal Audit

Find tests that consume maintenance cost without protecting meaningful behavior. Do not equate line count with value and do not call a test useless without evidence.

## Match the requested mode

- **Audit:** report candidates; do not edit.
- **Prove:** run controlled mutation experiments; do not edit the project.
- **Cleanup:** delete or rewrite only evidence-backed candidates.
- **PR:** complete cleanup, validation, independent review, commit, push, and open the PR.

Do not advance beyond the user's requested mode.

## Value standard

A valuable test fails when a meaningful behavior it claims to protect breaks. Prefer behavior observable by callers over implementation shape.

For each candidate, answer:

1. What meaningful defect should this test catch?
2. What controlled production mutation represents that defect?
3. Does the test fail under that mutation?
4. Would a harmless refactor break the test?
5. Does a stronger or cheaper check already catch the same defect?

A surviving mutation proves only that the assertion misses that failure mode. It does **not** prove an entire file can never catch any defect. Classify and change assertions individually unless the whole suite covers dead code.

## Start safely

1. Read repository instructions and testing philosophy.
2. Find the relevant build, typecheck, lint, and full-test commands.
3. Inspect `git status`; never overwrite unrelated work.
4. For mutation experiments, use a temporary detached worktree or equivalent isolated copy.
5. Establish a passing baseline for the target tests before mutating anything.
6. Read test comments and relevant git history before deleting regression coverage.

Keep mutation logs outside the repository unless the user requests committed evidence.

## Candidate signals

Investigate tests that assert only:

- exported symbol, method, command, tool, route, or registry existence
- exact tool/section/field counts
- `toBeDefined`, non-empty strings, constructor defaults, or object identity
- prompt phrases, formatting, XML wrappers, ordering, or snapshots
- TypeScript errors wrapped in a runtime test
- mock call counts without arguments or side effects
- copied constants or metadata against another copied constant
- private state, source text, regular expressions over implementation files
- dead or unregistered code with no production consumers
- behavior already covered at a stronger composition or boundary layer

These are leads, not automatic deletions.

## Mutation protocol

Run one mutation at a time:

1. State the claimed invariant.
2. Apply the smallest meaningful defect in the isolated worktree.
3. Run the narrow target test.
4. Run any stronger overlapping check, typecheck, or integration test.
5. Record exit codes and the relevant failure/pass output.
6. Restore the mutation before starting another.

Useful experiments include:

- rename or remove one registered capability while preserving an exact count
- forward the wrong tenant, ID, token, body, or option through a proxy
- set a configuration to the opposite value named by the test
- add contradictory prompt guidance while preserving expected phrases
- change a readonly/exported type while running runtime tests and typecheck separately
- remove a supposedly dead implementation and its tests, then run build and full tests

Never use a mutation that cannot affect real behavior merely to manufacture a passing test.

Use [the evidence template](references/evidence-template.md) to record results.

## Decide per assertion

### DELETE

Delete only when evidence shows no unique behavioral signal, such as:

- the implementation is unused and its removal passes build and the full suite
- the assertion is enforced more precisely by the compiler or schema tooling
- a stronger test kills the same mutation while this assertion survives it
- the assertion checks representation, count, formatting, or definedness with no consequential invariant
- the same branch and outcome are covered at a more realistic boundary

Delete unused helpers and imports with the assertion. Do not delete production code unless the user explicitly requested it.

### REWRITE

Rewrite when the behavior matters but the current assertion survives a relevant defect. Examples:

- response-only route test → assert forwarded identity, IDs, body, and side effect
- exact tool count → assert required names, forbidden names, uniqueness, and optional gates
- config object exists → assert the consequential option value or observable lifecycle
- copied diagnostic metadata → derive from or compare with runtime definitions
- source-regex assertion → exercise the exported protocol or state transition
- prompt phrase inventory → use an evaluation/scenario test where available

After rewriting, rerun the original mutation and verify the new test fails.

### KEEP

Keep tests with unique signal for:

- authentication, authorization, tenant isolation, privacy, signatures, and validation boundaries
- persistence, money, rate limits, retries, races, cancellation, ordering, streaming, and durable replay
- externally consumed wire formats and compatibility contracts
- meaningful success and error branches not covered elsewhere
- historical regressions whose mutation still kills the test

Prompt rules need special care: an additive contradiction can show an inventory is incomplete, while scoped phrase assertions may still catch removal of a unique regression rule. Keep the minimal unique guards until behavioral evaluations replace them.

## Cleanup workflow

1. Apply only evidence-backed deletions and rewrites.
2. Format and typecheck.
3. Run targeted tests.
4. Run the complete relevant suite.
5. Rerun kill mutations for rewritten tests.
6. Run `git diff --check` and inspect the complete diff.
7. Ask a fresh-context reviewer to find accidental loss of unique coverage.
8. Address review findings and rerun affected checks.

Do not mark validation complete while checks fail.

## PR standard

The PR must state:

- what was deleted versus rewritten
- the controlled mutations and exact outcomes
- which stronger checks retained coverage
- which apparently brittle tests were deliberately kept and why
- full validation commands and counts

Avoid claiming universal proof. Say the removed assertion was redundant, false-positive, dead-code-only, or under-sensitive for the tested failure mode.
