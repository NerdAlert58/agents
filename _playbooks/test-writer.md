# Playbook: Test-Writer

**Purpose:** Draft failing tests against the acceptance criteria in a SPEC before any implementation begins (initial mode), or add tests for newly-discovered uncovered code paths during implementation (incremental mode). Tests are written in the repo's native test framework and produced in a runnable state so the runner can pick them up and assign IDs.

**When to invoke:**
- **Initial mode:** After the spec-producer's `SPEC.md` exists and the coordinator has assigned a Tier 1 (or Tier 2 where a separate test-author dispatch is requested) but before implementation begins.
- **Incremental mode:** When the implementer raises an uncovered-path gap per implementer-challenge discipline (`_planning/test-gated-workflow.md` §4 Tier 1 step 6). The implementer has paused; you are dispatched to add tests for the specific scenario(s) they identified.

---

## Inputs (required)

- `<working_repo>/SPEC.md` — acceptance criteria the tests must target. In initial mode this is the primary driver; in incremental mode it is still required as the source of truth the new tests must remain consistent with.
- `<working_repo>/` — root of the repo so you can identify the native test framework, existing test layout, and language conventions.

## Inputs (optional)

- `<working_repo>/ARCHITECTURE.md` — documented failure modes, invariants, concurrency model. If present, treat its failure-mode list as mandatory coverage targets.
- `<working_repo>/TESTBOOK.md` — current test inventory. Read in incremental mode to avoid duplicating an existing test; read in initial mode to learn the repo's naming and topic conventions.
- `<working_repo>/TESTBOOK-ids.yml` — stable-ID mapping. The runner assigns IDs; you do not. Read only to confirm conventions and avoid collision with native test names.
- **Incremental-mode dispatch note** — the implementer's gap statement: which code path, why no existing test covers it, what behavior must be pinned. The dispatcher passes this in the prompt body.

---

## Process

### Step 0 — Determine mode

Read the dispatch prompt. If it names a specific uncovered scenario raised by the implementer, you are in **incremental mode** — skip to Step 5. Otherwise you are in **initial mode** — start at Step 1.

### Step 1 — Read SPEC.md in full (initial mode)

Extract:
- Each numbered acceptance criterion.
- Every documented failure mode (explicit or implied).
- Stated boundary values, limits, ranges, timeouts.
- Whether the component is **stateful** (concurrent-access scenarios become mandatory if so).
- Any input shape: required/optional fields, types, allowed null/empty handling.

If SPEC.md is silent on any of: failure modes, boundaries, null/empty handling — record the silence explicitly in a `## Coverage Gaps` section of your draft notes and proceed using best-judgment defaults. **Do not** invent acceptance criteria; mark the assumption.

### Step 2 — Survey the test framework and conventions

- Identify the native runner (`go test`, `pytest`, `jest`, etc.).
- Locate existing test files; mirror their directory layout, naming convention, helper imports, and assertion style.
- Note any shared fixtures, table-driven test patterns, or builders already in use — prefer reusing them over inventing new scaffolding.

### Step 3 — Enumerate required test cases against the coverage checklist

For each acceptance criterion, generate test cases covering — at minimum — all of these dimensions that apply:

1. **Happy path** — the criterion's stated success behavior, with a representative valid input.
2. **Documented failure modes** — every failure mode listed in SPEC.md and/or ARCHITECTURE.md gets at least one test.
3. **Boundary values** — min, max, just-below-min, just-above-max, exactly-at-threshold. For each numeric or sized input.
4. **Empty / null / malformed inputs** — empty string, empty collection, null/None/nil, wrong type, structurally invalid payload. Skip a dimension only if the input type genuinely makes it impossible (e.g., a non-nullable primitive in a typed language) and note the skip.
5. **Concurrent-access scenarios** — only if the component is stateful. Cover: two writers racing, reader-during-write, idempotency of retried operations, lock/transaction boundary behavior.

A criterion with no failure modes, no boundaries, no inputs, and no state should still produce at least one happy-path test plus one negative case (the obvious wrong-shape input).

### Step 4 — Write the tests

For each enumerated case:
- Use a **descriptive native test name** that maps cleanly to one sentence of intent (e.g., `TestPushToMasterRejected`, `test_login_with_expired_token_returns_401`). Names will become `topic` lines in TESTBOOK.md.
- Make the test **fail meaningfully** against the current (un-implemented or partially-implemented) code. A test that errors at import time or panics on a missing symbol is fine — it is still a failure, and it documents the missing surface. A test that silently passes against missing code is unacceptable.
- Keep each test focused on **one assertion of behavior**. Table-driven tests are fine, but each row asserts one named scenario.
- Use the existing fixture / builder patterns from Step 2.
- **Do NOT add a `TESTBOOK-LOCK` header.** Locking is the gatekeeper's or implementer's responsibility after grading (see `_planning/test-gated-workflow.md` §8).
- **Do NOT edit `TESTBOOK.md` or `TESTBOOK-ids.yml`.** The runner picks up new native tests and assigns IDs.

Then skip to Step 6.

### Step 5 — Incremental-mode write (single-scenario)

You were dispatched with a specific uncovered scenario. Constraints:

- **Stay narrow.** Write tests for the named scenario only. Do not opportunistically expand coverage of unrelated areas — that re-opens the lock surface and is out of scope.
- **Re-read the relevant SPEC section** to confirm the new test remains consistent with existing acceptance criteria. If the new scenario contradicts the SPEC, stop and report — this is a SPEC issue, not a test-writing issue.
- **Read TESTBOOK.md / TESTBOOK-ids.yml** to confirm no existing test already covers the scenario under a name you missed. If one does, report that and do not duplicate.
- **Apply the same coverage dimensions from Step 3** to *this scenario only* (happy path of the new path; failure modes of the new path; boundaries; null/empty; concurrent if stateful). Most incremental dispatches produce 1–4 tests, not a full suite.
- **Do not weaken or modify any existing locked test.** If you believe an existing test is wrong, stop and report — that is a lock-refresh decision for the gatekeeper, not a test-writer action (see `_planning/test-gated-workflow.md` §13 anti-goal: "Test-writer learned helplessness" — push back; do not silently relax coverage).
- Same no-lock, no-TESTBOOK-edit rules as Step 4.

### Step 6 — Verify the tests fail for the right reason

Run the native test command (or the repo's `tools/testbook-run` if present). Confirm:
- Every new test **fails** (or errors at compile / collection because the symbol does not yet exist — that is an acceptable failure mode pre-implementation).
- No new test **passes** against currently-missing implementation. A passing test pre-implementation is a sign the test does not actually exercise the intended behavior; fix it.

Record the run output (or paste of the failing summary) in your return summary so the dispatcher can confirm.

### Step 7 — Write the return summary

Return to the dispatcher a brief summary covering:
1. Mode (initial vs. incremental).
2. Files created / modified (absolute paths).
3. Count of new tests added, grouped by criterion or scenario.
4. Confirmation each new test failed in Step 6 and why (missing impl, wrong return, etc.).
5. Any **Coverage Gaps** noted in Step 1 — silences in SPEC where you made assumptions.
6. Any pushback: places you declined to weaken an existing test or where SPEC contradicts the requested scenario.

---

## Outputs

- **Path(s):** native test files under the repo's existing test layout. Exact paths depend on the language and repo conventions discovered in Step 2. Examples:
  - Go: `<working_repo>/<pkg>/<feature>_test.go`
  - Python: `<working_repo>/tests/test_<feature>.py`
  - JS/TS: `<working_repo>/__tests__/<feature>.test.ts` or co-located `*.test.ts`
- **Required structure:** files compile/parse and are discoverable by the native runner. Each test has a descriptive name (Step 4). No `TESTBOOK-LOCK` header. No edits to `TESTBOOK.md` or `TESTBOOK-ids.yml`.
- **Not produced by this playbook:** the lock line, TESTBOOK entries, ID assignments, the gatekeeper rubric grade.

---

## Pass criteria (boolean rubric for Jasnah)

- [ ] One or more new native test files exist (initial mode) OR the existing test file(s) named in the incremental dispatch are extended with new test(s) (incremental mode).
- [ ] Every acceptance criterion in SPEC.md has at least one corresponding test (initial mode only).
- [ ] For each criterion / scenario, every applicable coverage dimension is represented: happy path, documented failure modes, boundary values, empty/null/malformed inputs, concurrent-access (if stateful). Skipped dimensions are explicitly justified in the return summary.
- [ ] All new tests fail (or error pre-symbol) when run against current code. No new test passes vacuously.
- [ ] Each new test has a descriptive name that reads as a one-sentence intent statement.
- [ ] No `TESTBOOK-LOCK` header is present in any file produced by this playbook.
- [ ] `TESTBOOK.md` and `TESTBOOK-ids.yml` are unmodified.
- [ ] No existing locked test was weakened, deleted, or behaviorally relaxed.
- [ ] Return summary lists files, counts, failure confirmation, coverage gaps, and any pushback.

---

## Common pitfalls

- **Writing tests that pass.** Pre-implementation, every new test must fail or error. A green test against missing code is almost always asserting nothing meaningful.
- **Skipping null/empty because "the type prevents it."** Often the type does not prevent it at the boundary (deserialization, FFI, JSON parse). Skip only after confirming.
- **Forgetting concurrent-access for stateful components.** If the SPEC describes any persistent state — cache, DB, file, in-memory map shared across requests — concurrent-access tests are mandatory, not optional.
- **Inventing acceptance criteria.** If SPEC.md is silent, record the silence in Coverage Gaps and assume conservatively — do not back-fill the SPEC from your own model of "what the feature probably should do."
- **Adding a TESTBOOK-LOCK header.** Locking is downstream of gatekeeper grading. Producing a pre-locked test bypasses the review step.
- **Editing TESTBOOK.md directly.** The runner owns that file. Hand-edits collide with runner output.
- **Incremental-mode scope creep.** You were dispatched for one scenario. Adding "while I'm here" tests re-opens the locked surface and forces the gatekeeper to re-review unrelated coverage.
- **Rubber-stamping the implementer's challenge.** If the implementer claims a code path is uncovered but a search shows it is, push back. If the implementer wants a test weakened because their code does not pass it, refuse and surface to the user. See `_planning/test-gated-workflow.md` §13.
- **Mirroring implementation choices in tests.** Tests assert behavior described by the SPEC, not the structure the implementer happens to choose. A test that breaks when the implementer refactors a private helper is testing the wrong layer.

---

## Promotion notes

*(Track each invocation here as the playbook is used. When usage hits 3+ real assignments and rework hits 2+ instances, this playbook is a promotion candidate to a dedicated Test-Writer persona.)*

| Date | Mode | Working repo | Tests added | Rework needed? |
|---|---|---|---|---|
| *(empty — populate on first invocation)* | | | | |
