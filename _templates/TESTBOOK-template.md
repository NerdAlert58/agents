# Testbook — <REPO-NAME>

<!--
TESTBOOK.md is the single living document tracking every test in this repo.
It is written/updated by the runner (tools/testbook-run or equivalent) on
every suite execution, and read by humans and agents to understand current
test state without reading raw test framework output.

Full specification: ~/Git/agents/_planning/test-gated-workflow.md

Quick rules:
- IDs are stable. Once 002 is assigned, 002 always refers to that test
  (or its tombstone). Never recycle IDs.
- Numbering is monotonically increasing across the entire file.
- Retired tests leave a [-] tombstone, never deleted outright.
- The runner is the only thing that should overwrite the Last-run header
  and the per-test pass/fail markers. Humans/agents add new tests by
  editing the body, and the runner picks them up on next run.
-->

Last run: NEVER — TESTBOOK initialized but no runs recorded yet
State hash: —
Status of last run: —

| Tier | Open | Done | Regressed |
|------|------|------|-----------|
| 1    | 0    | 0    | 0         |
| 2    | 0    | 0    | 0         |
| 3    | 0    | 0    | 0         |
| 4    | 0    | 0    | 0         |

---

## Marker legend

| Marker | Meaning |
|--------|---------|
| `[✓]`  | PASS in latest run |
| `[✗]`  | FAIL in latest run |
| `[+]`  | New test this run |
| `[-]`  | Retired (tombstone, never renumbered) |
| `⚠ regression` | Was PASS in prior run, now FAIL |
| `⚠ waived`     | User explicitly waived this regression — see commit message |

---

## How to add a test

1. Write the native test in your repo's test framework (e.g., `go test`, `pytest`).
2. Run the testbook runner — it auto-detects the new test and assigns the next available ID.
3. Open `TESTBOOK-ids.yml` and fill in `feature` and `topic` for the new ID.
4. Run the runner again; the new entry appears in TESTBOOK.md with `[+]` marker.

For Tier 1 work: the test must be locked via `TESTBOOK-LOCK` header in the test file before implementation begins. See the test-gated-workflow design doc for full ceremony.

---

## Features

<!--
Body organization:

## Feature: <feature name>   [Tier 1|2|3|4]   [opened: YYYY-MM-DD]   [status: in-progress|done|regressed]

### Topic: <topic name>
- [marker] NNN — short test description

The runner overwrites markers and the Last-run header on every run.
Humans/agents add new Feature/Topic sections and new test lines.

Example skeleton (delete once you have real content):

## Feature: User authentication   [Tier 1]   [opened: 2026-05-11]   [status: in-progress]

### Topic: Login flow
- [ ] 001 — successful login with valid credentials returns 200
- [ ] 002 — failed login with wrong password returns 401
- [ ] 003 — login attempt against deleted account returns 404

### Topic: Session management
- [ ] 004 — session token expires after configured TTL
- [ ] 005 — expired session token returns 401 on protected route
-->

*(no features tracked yet — add your first section under this line)*

---

## Waived regressions

<!--
When the user explicitly waives a regression at merge time, the runner
records it here so it doesn't get forgotten. Each entry includes:
- Test ID
- Date waived
- Reason (from commit message)
- Commit SHA

Example:
- 014 — waived 2026-05-15 — "accepted: API contract change, downstream consumers notified" — abc1234
-->

*(no waivers recorded)*
