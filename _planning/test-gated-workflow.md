# Design: Test-Gated Workflow

**Status:** DRAFT — under review
**Author:** Holdy (drafted in collaboration with Scott)
**Created:** 2026-05-11
**Scope:** Phase 1 design for Scope A (assignment / project repos). Scope B (agents repo eval overlay) deferred.

---

## 1. Purpose

Prevent the failure mode of agents marking work "done" when the work is not functional. Bind every non-trivial task to a written, agent-reviewed, user-locked test set that must pass before completion is recognized. Aggregate test results into a single living document (`TESTBOOK.md`) that grows with the codebase.

## 2. Non-goals

- **Replacing existing test frameworks.** `go test`, `pytest`, etc. continue to be the native runners. The Testbook is an aggregation layer on top.
- **Mandating strict TDD cadence.** Red-green-refactor as a habit is fine; the gate is "tests exist, are reviewed, and pass before completion is claimed."
- **Eliminating Jasnah's judgment from completion calls.** Jasnah still grades the boolean rubric; the test suite gives her objective inputs.

## 3. Severity tiers

Picard assigns a tier at task open. Recorded in the task SPEC and in `TESTBOOK.md` so it's auditable — "Picard called this Tier 2 and it bit us" is a recalibration signal.

### Tier 1 — Full ceremony
**For:** substantial features, bugfixes touching business logic, anything Reacher flagged as HARD gate, anything where regression matters.

**Flow:**
1. Reacher (or user) produces SPEC.md with acceptance criteria.
2. **Test-writer agent** (`general-purpose` dispatched with the test-writer playbook) drafts failing tests against the criteria.
3. **Jasnah** grades the test set against a coverage checklist:
   - Happy path covered?
   - Documented failure modes (from SPEC and/or Halliday's ARCHITECTURE.md) covered?
   - Boundary values covered?
   - Empty/null/malformed inputs covered?
   - Concurrent-access scenarios covered (if stateful)?
4. **Implementer agent** reviews tests and may flag missing cases.
5. Test-writer agent accepts the suggestions or pushes back with rationale.
6. **User adjudicates** if disputed. Default lean: keep suggested additions if relevant.
7. Tests committed with **TESTBOOK-LOCK** marker (see §7).
8. Implementer writes code.
9. Implementer runs the suite locally; iterates until tests pass.
10. **Verification-before-completion:** runner output drives the "done" claim, not agent self-report. Agent never writes "done"; the runner does.
11. STATUS reflected in `TESTBOOK.md`.
12. **Pre-merge:** full suite runs when user calls for it (Picard prompts at merge gate).

### Tier 2 — Light ceremony
**For:** small features, refactors with low blast radius, internal helpers, dependency bumps with behavior tests.

**Flow:**
1. SPEC may be a one-liner.
2. Implementer writes tests AND code (no separate test-writer dispatch).
3. No Jasnah review of tests.
4. Verification-before-completion still applies — runner output drives "done."
5. Tests still land in `TESTBOOK.md` with stable IDs.
6. Pre-merge suite still runs if user calls for it.

### Tier 3 — Trivial
**For:** typo fixes, doc-only changes, config tweaks with no functional impact, README edits.

**Flow:**
1. No tests required.
2. Reason recorded in commit message: `Tier 3 — trivial: <why>`.
3. Pre-merge suite still runs (catches accidental functional breakage).
4. If a Tier 3 commit causes regressions in the suite, it gets retroactively reclassified and Picard's tier-decision rule is re-examined.

### Picard's tier-assignment rule
Picard chooses tier at task open based on:
- Does the change alter executable behavior? → **Tier 3** if no
- Is blast radius >1 file or cross-system? → **Tier 1**
- Is it user-facing or part of an API contract? → **Tier 1**
- Otherwise → **Tier 2** (default)

The choice and its rationale are recorded in the task entry. Tier may be upgraded mid-task but never downgraded without user approval.

## 4. TESTBOOK.md — format spec

Single file per repo, lives at repo root.

### Header
```markdown
# Testbook — <repo name>

Last run: 2026-05-11T09:15:00Z — 18/20 PASS · 2 FAIL · 1 new · 1 regression vs prior

| Tier | Open | Done | Regressed |
|------|------|------|-----------|
| 1    | 3    | 12   | 0         |
| 2    | 1    | 4    | 1         |
| 3    | 0    | —    | 0         |
```

### Body — hierarchical
```markdown
## Feature: <feature name>   [Tier 1]   [opened: 2026-05-09]   [status: in-progress]

### Topic: <topic name>
- [✓] 001 — push to feature branch succeeds
- [✗] 002 — push to master rejected   ⚠ regression (was PASS at 2026-05-10T22:00:00Z)
- [✓] 003 — --no-verify bypasses
- [+] 004 — newly added: push to main also rejected

### Topic: <topic name>
- [✓] 005 — fresh clone + git config command activates hook
- [-] 006 — retired 2026-05-11: superseded by 004
```

### Marker legend
| Marker | Meaning |
|--------|---------|
| `[✓]`  | PASS in latest run |
| `[✗]`  | FAIL in latest run |
| `[+]`  | New test this run |
| `[-]`  | Retired (tombstone, never renumbered) |
| `⚠ regression` | Was PASS in prior run, now FAIL |

### Stable-ID rule
- IDs never recycle. Once `002` is assigned to a test, `002` always refers to that test (or its tombstone).
- Numbering is monotonically increasing across the entire file.
- When tests are retired, they leave a tombstone with a `retired YYYY-MM-DD: reason` note.

### Output
The runner prints the entire `TESTBOOK.md` to terminal on every run, with the latest results applied. Reading it should feel like reading a familiar document that grows over time — additions are visible as additions, regressions are visible as regressions.

## 5. Runner abstraction

Per-repo runner script: `tools/testbook-run` (or repo-idiomatic equivalent).

### Responsibilities
- Wraps the repo's native test framework (`go test`, `pytest`, etc.)
- Reads `TESTBOOK-ids.yml` for stable-ID → native-test-name mapping
- Parses native results
- Writes updated `TESTBOOK.md`
- Prints `TESTBOOK.md` to terminal
- Returns exit code `0` if no failures AND no regressions; `1` otherwise

### ID mapping sidecar — `TESTBOOK-ids.yml`
```yaml
- id: 001
  native: TestPushToFeatureBranchSucceeds
  feature: Pre-push hook
  topic: Branch protection
  tier: 1
  added: 2026-05-09
- id: 002
  native: TestPushToMasterRejected
  feature: Pre-push hook
  topic: Branch protection
  tier: 1
  added: 2026-05-09
```

When a new native test is detected with no mapping, the runner assigns the next available ID and prompts the user (or the test-writer agent on subsequent commits) to fill in `feature` and `topic`.

### Phase-1 deliverables
- Go runner (your default language) — reference implementation
- Python runner — added when Gauntlet tasks require it
- Shared spec for the `TESTBOOK.md` and `TESTBOOK-ids.yml` formats so future runners conform

## 6. Pre-merge prompt

Picard's playbook gains a behavior: at the merge gate of any task (Tier 1 or 2), Picard prompts the user:

> "Run the regression suite before merging? (recommended)"

If user agrees, runner executes. If any regression appears, merge is blocked pending resolution. Override via explicit "merge anyway, accept regression" in commit message — logged in `TESTBOOK.md` as a waived regression.

This is **prompt-based, not hook-based** per your preference for explicit control. The existing `.githooks/pre-push` continues to handle branch protection separately.

## 7. Lock mechanism — convention + hook

### Convention
Locked test files carry a header line within the first 10 lines of the file:
```
// TESTBOOK-LOCK: jasnah-approved 2026-05-11 hash:abc123def456...
```

(or `# TESTBOOK-LOCK:` for Python / shell, `<!-- TESTBOOK-LOCK: -->` for markdown.)

The `hash` is a SHA-256 of the file content excluding the lock line itself (chicken-and-egg avoidance).

### Pre-commit hook
Added at `.githooks/pre-commit` (companion to existing `pre-push`).

Behavior:
1. For each staged file matching the test glob (configured per repo)
2. If file has `TESTBOOK-LOCK` header:
   - Compute current content hash (excluding lock line)
   - Compare to declared hash
   - **If mismatch:** reject the commit with explanatory message
3. If no `TESTBOOK-LOCK` header: allow (Tier 2 / Tier 3 tests are unlocked)

### Refreshing a lock
If a locked test genuinely needs to change (the spec evolved, a bug was found in the test itself), the agent must:
1. Re-dispatch Jasnah to grade the proposed change
2. Update the lock line with new date and new hash
3. Commit normally

This makes test-weakening a **visible, auditable event** — not silent drift.

### Override
`git commit --no-verify` bypasses the hook. Use is logged in the commit message; surfaced in PR review.

## 8. Agent flow summary (Tier 1)

```
User opens task
   ↓
Picard assigns Tier 1 + records in TESTBOOK.md
   ↓
Reacher → SPEC.md
   ↓
Test-writer agent → draft tests (failing)
   ↓
Jasnah → grade tests against rubric checklist
   │
   ├─ PASS: proceed
   └─ FAIL: test-writer iterates
   ↓
Implementer agent → review tests, flag missing cases
   │
   ├─ Test-writer agrees: add tests
   └─ Test-writer disagrees: USER ADJUDICATES
   ↓
Tests committed with TESTBOOK-LOCK
   ↓
Implementer → write code
   ↓
Runner → suite runs locally
   ↓
TESTBOOK.md updated; printed to terminal
   ↓
Jasnah grades completion against rubric (uses runner output as evidence)
   ↓
User calls for pre-merge run
   ↓
If clean: merge; if regression: block
```

## 9. Implementation plan — phases

| Phase | Deliverable | Owner |
|-------|-------------|-------|
| 0 | This design doc, reviewed and approved | Scott + Holdy |
| 1 | `TESTBOOK.md` template + format playbook in `_playbooks/` | Holdy |
| 2 | Picard playbook updates: tier assignment, test-gated handoff, pre-merge prompt | Holdy → Picard v0.3 |
| 3 | Test-writer playbook (for dispatching `general-purpose` as test author) | Holdy |
| 4 | Jasnah rubric checklist for test review | Holdy → Jasnah v0.2 |
| 5 | Go runner reference implementation in a sample repo | Scott |
| 6 | Pre-commit lock hook | Scott |
| 7 | Pilot run on one Gauntlet task; capture pain points | Scott |
| 8 | Revise playbook based on pilot | Holdy |
| 9 | (Deferred) Scope B overlay — agents-repo eval system mapped onto Testbook format | Holdy |

## 10. Open questions

**OQ-1: TESTBOOK per repo or per assignment within a multi-repo Gauntlet?**
Default: per repo. Revisit if it becomes unwieldy.

**OQ-2: Retired-test rationale — inline tombstone or separate file?**
Default: inline tombstone. Could move to `RETIRED-TESTS.md` if it grows.

**OQ-3: How does Scope A interact with the agents-repo eval sets (Scope B)?**
Deferred to Phase 9. The agents repo already has `<agent>-evals/` directories; the overlay design will probably treat each scenario as a numbered test in a Testbook-shaped doc.

**OQ-4: Regression budget — is *any* regression a merge blocker, or is there a tolerance?**
Default lean: **any regression blocks merge unless explicitly waived in commit message with reason.** Waived regressions are tracked in `TESTBOOK.md` so they don't get forgotten.

**OQ-5: What happens when a test is wrong (false fail) vs. when the code is wrong?**
The test-writer agent and implementer agent disagree → user adjudicates. If user determines the test is wrong, the lock is refreshed via the standard flow. Documented in commit history.

**OQ-6: What's the "happy path" of a Tier 1 task that takes 2 weeks?**
Long tasks may need intermediate Jasnah grading at sub-milestone points. Picard's playbook should include this. Out of scope for Phase 1; revisit.

## 11. Success criteria

Before declaring this workflow successful, we should observe:

1. **No agent marks a task "done" without TESTBOOK runner output backing the claim** — measured by review of completed tasks for the next 4 weeks.
2. **At least one regression caught at pre-merge that would have shipped without this workflow** — proves the persistent suite has value.
3. **Pilot run on one Gauntlet task completes without abandoning the workflow mid-task** — proves the overhead is tolerable.
4. **TESTBOOK.md is referenced in at least 3 design conversations** — proves it has become the familiar document we designed it to be.

If any of these fail after 4 weeks, revisit the design — don't add ceremony to compensate.

## 12. Anti-goals (things to watch out for)

- **TESTBOOK rot.** If the file becomes a wall of yellow/red that everyone ignores, the workflow has failed. Discipline around tombstoning irrelevant tests matters more than discipline around adding new ones.
- **Tier inflation.** Picard calling everything Tier 1 to be safe will burn time on trivial work. Audit tier assignments monthly.
- **Lock-bypass habit.** If `--no-verify` becomes routine, the lock is theater. Surface these in PR review and require justification.
- **Test-writer learned helplessness.** If the test-writer agent always defers to the implementer's challenges, it becomes a rubber stamp. Hold the line: test-writer should push back when right, and user adjudicates.
