# Design: Test-Gated Workflow

**Status:** DRAFT — under review
**Author:** Holdy
**Created:** 2026-05-11
**Scope:** Phase 1 design for Scope A (assignment / project repos). Scope B (agent-repo eval overlay) deferred.

This document defines a portable, role-based workflow. It is not tied to any specific bootcamp, organization, or agent roster. A reference implementation lives in this repository (see §2), but the workflow itself is intended as a toolkit applicable to greenfield and brownfield work alike.

---

## 1. Purpose

Prevent the failure mode of an implementer (human or agent) marking work "done" when the work is not functional. Bind every non-trivial task to a written, peer-reviewed, user-locked test set that must pass before completion is recognized. Aggregate test results into a single living document (`TESTBOOK.md`) that grows with the codebase.

## 2. Roles vs. role-fillers

This workflow defines abstract **roles**. The roles are the contract. How each role is filled is implementation-dependent.

| Role | Responsibility |
|------|----------------|
| **Coordinator** | Assigns severity tier at task open; tracks lifecycle; prompts at gate transitions; records auditable decisions |
| **Spec-producer** | Converts an ambiguous task description into structured acceptance criteria the test-writer can target |
| **Test-writer** | Drafts failing tests against the acceptance criteria before any implementation begins |
| **Implementer** | Reviews tests; writes code; raises gaps when implementation requires uncovered behavior |
| **Completion-gatekeeper** | Reviews test sets for coverage; grades completion against rubric; refuses "done" without evidence |

In the **reference implementation** living in this repository, these roles map as follows:

| Role | Reference filler |
|------|------------------|
| Coordinator | Picard |
| Spec-producer | Reacher |
| Test-writer | `general-purpose` agent dispatched with the test-writer playbook |
| Implementer | `general-purpose` agent dispatched with the implementer playbook |
| Completion-gatekeeper | Jasnah |

Other valid implementations: a solo developer filling all five roles personally; a team mapping them to humans; a different agent ensemble dispatching different specialists. The workflow does not depend on these specific agents — only on the roles being filled by someone (or something) capable.

## 3. Non-goals

- **Replacing existing test frameworks.** `go test`, `pytest`, etc. continue to be the native runners. The Testbook is an aggregation layer on top.
- **Mandating strict TDD cadence.** Red-green-refactor as a habit is fine; the gate is "tests exist, are reviewed, and pass before completion is claimed."
- **Eliminating the completion-gatekeeper's judgment from completion calls.** The gatekeeper still grades the boolean rubric; the test suite provides objective inputs.

## 4. Severity tiers

The coordinator assigns a tier at task open. The tier is recorded in the task SPEC and in `TESTBOOK.md` so it's auditable — "Tier 2 was assigned and it bit us" is a recalibration signal worth seeing.

### Tier comparison

| Tier | Separate test-writer | Gatekeeper test review | Implementer-challenge discipline | Tests locked | Pre-merge gate |
|------|---|---|---|---|---|
| **1** | ✓ | ✓ | ✓ | ✓ | ✓ |
| **2** | — (implementer writes) | ✓ | ✓ | ✓ | ✓ |
| **3** | — (implementer writes) | — (implementer self-check) | ✓ | optional | ✓ |
| **4** | — (no tests) | — | — | — | ✓ (catches drift) |

### Tier 1 — Full ceremony

**For:** substantial features, bugfixes touching business logic, any code that calls AI/LLM APIs (default), anything the spec-producer flagged as a HARD gate, anything where regression matters.

**Flow:**
1. Spec-producer → SPEC.md with acceptance criteria.
2. **Test-writer** drafts failing tests against the criteria.
3. **Completion-gatekeeper** grades the test set against the coverage checklist:
   - Happy path covered?
   - Documented failure modes (from SPEC and/or any ARCHITECTURE.md) covered?
   - Boundary values covered?
   - Empty / null / malformed inputs covered?
   - Concurrent-access scenarios covered (if stateful)?
4. Tests committed with **TESTBOOK-LOCK** marker (see §8).
5. **Implementer** begins writing code.
6. **Implementer-challenge discipline (continuous, not a one-shot gate):** if at any point during implementation the implementer needs to write code that no existing test exercises, they must:
   - **Stop** implementation of that path.
   - **Raise the gap** explicitly (in conversation with the user or via task-tracking surface).
   - Coordinate with test-writer to **add a new test**.
   - Re-dispatch the gatekeeper for **incremental review** of the new test.
   - **Re-lock** the test set.
   - **Resume** implementation.

   This converts "untested code" into a visible, auditable event rather than silent drift. It is the single most important discipline in this workflow.
7. **Verification-before-completion:** runner output drives the "done" claim, not agent self-report. The implementer never writes "done" — the runner output does.
8. **Pre-merge gate** runs if not already proven against current state (see §7).

### Tier 2 — Light ceremony, gatekeeper still reviews tests

**For:** small features, refactors with low blast radius, internal helpers, dependency bumps with behavior tests.

**Flow:**
1. SPEC may be a one-liner.
2. **Implementer** writes both tests AND code (no separate test-writer dispatch).
3. **Completion-gatekeeper** still grades the test set against the coverage checklist.
4. Tests committed with TESTBOOK-LOCK.
5. Implementer-challenge discipline applies (§Tier 1 step 6).
6. Verification-before-completion applies.
7. Pre-merge gate applies.

### Tier 3 — Light ceremony, no gatekeeper review

**For:** contributor work the user has signed off on at design time; routine work where the implementer's self-check is judged sufficient.

**Flow:**
1. Implementer writes both tests AND code.
2. **No gatekeeper review** of the test set (implementer self-checks against the same coverage checklist).
3. Tests may or may not be locked (implementer's call).
4. Implementer-challenge discipline applies.
5. Verification-before-completion applies — runner output drives "done," not self-report.
6. Pre-merge gate applies.

### Tier 4 — Trivial

**For:** typo fixes, doc-only changes, config tweaks with no functional impact, README edits.

**Flow:**
1. No tests required.
2. Reason recorded in commit message: `Tier 4 — trivial: <why>`.
3. Pre-merge suite still runs (catches accidental functional breakage from a "trivial" change that wasn't).
4. If a Tier 4 commit causes regressions in the suite, it gets **retroactively reclassified** and the coordinator's tier-decision rule is re-examined.

### Coordinator's tier-assignment rule

The coordinator chooses tier at task open based on:
- Does the change alter executable behavior? → **Tier 4** if no.
- Does the change introduce or modify code that calls AI/LLM APIs? → **Tier 1** unless an unusual case is explicitly justified in the task entry.
- Is blast radius >1 file or cross-system? → **Tier 1**.
- Is it user-facing or part of an API contract? → **Tier 1**.
- Has the user pre-signed off on this category of work? → may use **Tier 3**.
- Otherwise → **Tier 2** (default).

The choice and its rationale are recorded in the task entry. **Tier may be upgraded mid-task but never downgraded without explicit user approval.**

## 5. TESTBOOK.md — format spec

Single file per repo, lives at repo root.

### Header
```markdown
# Testbook — <repo name>

Last run: 2026-05-11T09:15:00Z — 18/20 PASS · 2 FAIL · 1 new · 1 regression vs prior
State hash: <sha256-of-branch-head + merge-base-with-target>
Status of last run: PASS

| Tier | Open | Done | Regressed |
|------|------|------|-----------|
| 1    | 3    | 12   | 0         |
| 2    | 1    | 4    | 1         |
| 3    | 0    | —    | 0         |
| 4    | 0    | —    | 0         |
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

## 6. Runner abstraction

Per-repo runner script: `tools/testbook-run` (or repo-idiomatic equivalent).

### Responsibilities
- Wraps the repo's native test framework (`go test`, `pytest`, etc.).
- Reads `TESTBOOK-ids.yml` for stable-ID → native-test-name mapping.
- Parses native results.
- Writes updated `TESTBOOK.md` — including the **state hash** (see §7) so future runs can detect "already proven."
- Prints `TESTBOOK.md` to terminal.
- Returns exit code `0` if no failures AND no regressions; `1` otherwise.

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

When a new native test is detected with no mapping, the runner assigns the next available ID and prompts the user (or the test-writer on subsequent commits) to fill in `feature` and `topic`.

### Runner sourcing — per-project, no central reference

Each project repo authors its own runner in whatever language fits that project. The runner spec above is the contract; conformance is the only requirement. There is **no central reference implementation** and no canonical language. A Go project writes a Go runner; a Python project writes a Python runner; a shell-only repo writes a bash runner.

Rationale (decided 2026-05-11): we cannot predict what language any future project will use. A "reference runner" in one language would either get rewritten on every adoption (waste) or pressure projects to adopt a language they wouldn't otherwise choose (worse waste). The format spec above is the durable artifact; the runner is custom-fit per project.

## 7. Pre-merge gate — "already proven" optimization

At the merge gate of any task (Tier 1, 2, or 3), the coordinator prompts the user.

### State-hash mechanic

The runner records a **state hash** with every successful run in `TESTBOOK.md`. The hash combines:
- Current branch HEAD commit SHA
- Merge-base commit SHA with the target branch (typically master/main)

This captures both *"branch changed since last run"* and *"target branch advanced under us since last run"* — either condition invalidates the prior result.

### Prompt logic

**If current state hash matches the last-recorded successful run:**
```
Skip suite run? (last run at 2026-05-11T09:15Z covered current state; no changes detected since)
[Y] skip  [N] run anyway
```
Default action: skip. Override available with `--force` flag — recommended when paranoid, or when external factors changed (dependency version bumps not visible to git, infra config drift, etc.).

**If state has changed:**
The run is required. No skip option.

### Regression handling

If any regression appears in the run:
- Merge is **blocked** pending resolution.
- **The user has final say on every regression decision** — to maintain awareness, this is never delegated to an agent.
- Override path: user explicitly waives in commit message (`Waiving regression on tests 002, 014 — accepted because <reason>`). Waivers are recorded in `TESTBOOK.md` as a structured entry for future audit. They don't get forgotten.

This is **prompt-based with intelligent default**, not auto-hooked. The user retains explicit control over both whether to run and whether to merge with a regression. The existing `.githooks/pre-push` continues to handle branch protection separately.

## 8. Lock mechanism — convention + hook

### Convention

Locked test files carry a header line within the first 10 lines:
```
// TESTBOOK-LOCK: gatekeeper-approved 2026-05-11 hash:abc123def456...
```

Comment syntax varies by language: `//` (Go, JS, Rust), `#` (Python, shell, Ruby), `<!-- -->` (Markdown, HTML), etc.

The `hash` is a SHA-256 of the file content **excluding the lock line itself** (chicken-and-egg avoidance).

### Pre-commit hook

Added at `.githooks/pre-commit` (companion to the existing `pre-push`).

Behavior:
1. For each staged file matching the test glob (configured per repo).
2. If the file has a `TESTBOOK-LOCK` header:
   - Compute current content hash (excluding the lock line).
   - Compare to declared hash.
   - **If mismatch:** reject the commit with explanatory message.
3. If no `TESTBOOK-LOCK` header: allow (Tier 3 tests may be unlocked).

### Refreshing a lock

If a locked test genuinely needs to change (the spec evolved, a bug was found in the test itself), the implementer must:
1. Re-dispatch the completion-gatekeeper to grade the proposed change.
2. Update the lock line with new date and new hash.
3. Commit normally.

This makes test-weakening a **visible, auditable event** — not silent drift.

### Override

`git commit --no-verify` bypasses the hook. Use is logged in the commit message; surfaced in PR review. Habitual `--no-verify` use is itself a recalibration signal — see §13 anti-goals.

## 9. Agent flow summary (Tier 1)

```
User opens task
   ↓
Coordinator assigns Tier 1 + records in TESTBOOK.md
   ↓
Spec-producer → SPEC.md
   ↓
Test-writer → draft tests (failing)
   ↓
Completion-gatekeeper → grade tests against rubric
   │
   ├─ PASS: proceed
   └─ FAIL: test-writer iterates
   ↓
Tests committed with TESTBOOK-LOCK
   ↓
Implementer → write code
   ┊        ⤴
   ┊         │  (if implementer hits an uncovered code path)
   ┊         ├──→ Stop → raise gap → test-writer adds test
   ┊         │              → gatekeeper grades new test
   ┊         │              → re-lock → resume
   ┊         ↓
   ↓
Runner → suite runs locally → TESTBOOK.md updated → printed to terminal
   ↓
Completion-gatekeeper grades against rubric (uses runner output as evidence)
   ↓
User → pre-merge prompt (skipped if already proven for current state)
   ↓
If clean: merge.   If regression: USER decides waive or block.
```

## 10. Implementation plan — phases

| Phase | Deliverable | Owner |
|-------|-------------|-------|
| 0 | This design doc, reviewed and approved | User + Holdy |
| 1 | `TESTBOOK.md` template + format playbook in `_playbooks/` | Holdy |
| 2 | Coordinator playbook updates: tier assignment, test-gated handoff, pre-merge prompt | Holdy → Picard v0.3 (reference implementation) |
| 3 | Test-writer playbook (for dispatching `general-purpose` as test author) | Holdy |
| 4 | Completion-gatekeeper rubric checklist for test review | Holdy → Jasnah v0.2 (reference implementation) |
| 5 | ~~Go runner reference implementation~~ — **SKIPPED** per decision 2026-05-11. Per-project runner authored when needed; format spec in §6 is the durable artifact. | — |
| 6 | Pre-commit lock hook | User |
| 7 | Pilot run on one real task; capture pain points | User |
| 8 | Revise playbook based on pilot | Holdy |
| 9 | (Deferred) Scope B overlay — agent-repo eval system mapped onto Testbook format | Holdy |

## 11. Open questions

**OQ-1: TESTBOOK per repo or per assignment within a multi-repo project?**
Default: per repo. Revisit if it becomes unwieldy.

**OQ-2: Retired-test rationale — inline tombstone or separate file?**
Default: inline tombstone. Could move to `RETIRED-TESTS.md` if it grows.

**OQ-3: How does Scope A interact with agent-repo eval sets (Scope B)?**
Deferred to Phase 9. The reference-implementation agent repo already has `<agent>-evals/` directories; the overlay design will probably treat each scenario as a numbered test in a Testbook-shaped doc.

**OQ-4: Regression budget.**
**Resolved.** Any regression blocks merge unless explicitly waived in commit message. Waivers are logged in `TESTBOOK.md`. **The user has final say on every regression decision** to maintain awareness.

**OQ-5: What happens when a test is wrong (false fail) vs. when the code is wrong?**
If the implementer suspects a test is wrong, they raise it as a lock-refresh request. The gatekeeper grades the proposed change. The user adjudicates if disputed. Documented in commit history.

**OQ-6: Long tasks — Tier 1 work that spans multiple sessions or days.**
Long tasks may need intermediate gatekeeper grading at sub-milestone points. The coordinator's playbook should include this. Out of scope for Phase 1; revisit during pilot.

## 12. Success criteria

Before declaring this workflow successful, we should observe:

1. **No implementer marks a task "done" without runner output backing the claim** — measured by review of completed tasks for the next 4 weeks of active use.
2. **At least one regression caught at pre-merge that would have shipped without this workflow** — proves the persistent suite has value.
3. **Pilot run on one real task completes without abandoning the workflow mid-task** — proves the overhead is tolerable.
4. **TESTBOOK.md is referenced in at least 3 design conversations** — proves it has become the familiar document we designed it to be.

If any of these fail after 4 weeks of use, revisit the design — don't add ceremony to compensate for a workflow people don't trust.

## 13. Anti-goals (things to watch out for)

- **TESTBOOK rot.** If the file becomes a wall of yellow/red that everyone ignores, the workflow has failed. Discipline around tombstoning irrelevant tests matters more than discipline around adding new ones.
- **Tier inflation.** The coordinator calling everything Tier 1 to be safe will burn time on trivial work. Audit tier assignments periodically.
- **Lock-bypass habit.** If `--no-verify` becomes routine, the lock is theater. Surface bypasses in PR review and require justification.
- **Test-writer learned helplessness.** If the test-writer always defers to the implementer's challenges, it becomes a rubber stamp. The implementer-challenge step is meant to raise *gaps in coverage*, not to weaken existing tests. Test-writer should push back when right; user adjudicates.
- **Implementer-challenge fatigue.** If implementers stop raising gaps because "it's annoying to pause," the workflow has lost its load-bearing discipline. The coordinator should reinforce that pausing to raise a gap is the *correct* behavior, not the slow one.
