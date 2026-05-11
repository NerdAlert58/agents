# Playbook: testbook-init

## Purpose
Initialize `TESTBOOK.md` and `TESTBOOK-ids.yml` in a target repo so the test-gated workflow can begin operating. Discover any existing tests in the repo and seed the ID-mapping file with them at their pre-existing state.

## When to invoke
- Adopting the test-gated workflow in a new (greenfield) repo.
- Adopting the workflow in an existing (brownfield) repo that has tests but no TESTBOOK.
- Re-bootstrapping after a TESTBOOK was deleted or corrupted (rare; coordinate with user first).

## Inputs (required)
- **Target repo absolute path** — where to drop the files.
- **Test framework** — one of: `go`, `python`, `node`, `ruby`, `mixed`, `other`. Determines how the playbook discovers existing tests.
- **Test file glob(s)** — patterns identifying test files. Defaults if not specified:
  - `go`: `**/*_test.go`
  - `python`: `tests/**/*.py`, `**/test_*.py`
  - `node`: `**/*.test.js`, `**/*.spec.js`, `**/*.test.ts`, `**/*.spec.ts`
  - `ruby`: `spec/**/*_spec.rb`, `test/**/*_test.rb`

## Inputs (optional)
- **Feature/topic seed mapping** — if the user has a pre-existing mental model of how to group existing tests, accept a list like:
  ```
  feature "User auth" / topic "Login flow" → tests matching *Login*
  feature "Catalog" / topic "Read path" → tests matching *Get*Product*
  ```
- **Default tier** — for seeded tests when no other signal is available. Defaults to `2`.

## Process

1. **Verify target repo state.**
   - Confirm `<repo>/TESTBOOK.md` does not already exist. If it does, STOP and report — do not overwrite.
   - Confirm `<repo>/TESTBOOK-ids.yml` does not already exist. Same rule.

2. **Discover existing tests.**
   - For each test file matching the glob(s), parse out the native test names using the framework's conventions:
     - `go`: regex `func (Test[A-Z]\w+)\(`
     - `python` (pytest): regex `def (test_\w+)\(`
     - `node` (jest/mocha): regex `(?:it|test)\(['"]([^'"]+)['"]`
     - For other frameworks: ask the user for the discovery pattern.
   - Build a list of `{native_name, file_path}` pairs.

3. **Assign stable IDs.**
   - Starting at `1`, assign sequential IDs in discovery order (sort tests alphabetically by file path then native name for determinism).
   - Each entry gets: `id`, `native`, `tier` (from default or seed mapping), `added` (today's date), `locked: false`.

4. **Seed feature/topic if mapping provided.**
   - Apply the user-provided seed mapping to assign `feature` and `topic` where rules match.
   - For tests not covered by any rule, leave `feature` and `topic` as empty strings — surfaces them as a follow-up task.

5. **Write `TESTBOOK-ids.yml`** at `<repo>/TESTBOOK-ids.yml`:
   - Use the template at `~/Git/agents/_templates/TESTBOOK-ids-template.yml` as the comment header.
   - Populate the `tests:` list with the discovered entries.

6. **Write `TESTBOOK.md`** at `<repo>/TESTBOOK.md`:
   - Use the template at `~/Git/agents/_templates/TESTBOOK-template.md` as the starting structure.
   - Set `Last run: NEVER` and `State hash: —`.
   - Build the Features section by grouping seeded entries by feature → topic.
   - For each test: emit `- [ ] NNN — <native test name>` with a blank marker (not `[✓]` or `[✗]` — we haven't run anything yet; the runner will fill these on first execution).
   - For tests with empty `feature` / `topic`: put them under a placeholder section:
     ```markdown
     ## Feature: UNCATEGORIZED   [Tier 2]   [opened: YYYY-MM-DD]   [status: needs categorization]

     ### Topic: TBD
     - [ ] 0NN — <native name>
     ```

7. **Print summary to user:**
   ```
   TESTBOOK initialized at <repo>:
   - <N> tests discovered
   - <M> tests categorized into <F> features / <T> topics
   - <U> tests in UNCATEGORIZED — follow-up needed to assign feature/topic
   - Next: run the testbook runner to populate first-run results
   ```

8. **Return to dispatcher** a 5-line summary with the same data plus the two file paths.

## Outputs
- `<repo>/TESTBOOK.md` — written from template, populated with discovered tests, all markers blank.
- `<repo>/TESTBOOK-ids.yml` — written from template, `tests:` list populated.

## Pass criteria (boolean rubric for completion-gatekeeper)
- [ ] Both output files exist at the target paths.
- [ ] Neither file overwrote a pre-existing TESTBOOK.md or TESTBOOK-ids.yml without explicit user permission.
- [ ] Every discovered native test has exactly one corresponding entry in `TESTBOOK-ids.yml`.
- [ ] Every entry in `TESTBOOK-ids.yml` appears exactly once in `TESTBOOK.md`.
- [ ] IDs are monotonically increasing integers with no gaps and no duplicates.
- [ ] All markers in `TESTBOOK.md` are blank (`[ ]`), not `[✓]` / `[✗]` / `[+]` / `[-]`.
- [ ] Uncategorized tests are surfaced under an `UNCATEGORIZED` feature, not silently misfiled.
- [ ] The 5-line summary returned to the dispatcher includes counts of: total tests, categorized tests, features, topics, and uncategorized tests.

## Common pitfalls
- **Silent overwrite.** If a TESTBOOK already exists, the playbook must STOP, not merge. Merging existing TESTBOOK state with a fresh init is a separate, riskier playbook (not yet written).
- **ID gaps from "smart" sorting.** Always assign IDs in deterministic order. If discovery is non-deterministic, re-runs produce different ID assignments — defeats the stable-ID guarantee.
- **Inventing feature/topic categorization.** If the user hasn't provided seed rules, do NOT guess feature names from test name prefixes — surfaces as UNCATEGORIZED and asks the user. Wrong categorization is worse than absent categorization.
- **Treating pytest test classes as topics.** Test classes are an implementation detail of the framework, not a domain concept. Don't auto-map.
- **Marking discovered tests as passing.** Even if they pass today, the initial TESTBOOK has zero runs recorded. Markers are blank until the runner executes.

## Promotion notes
- New playbook as of 2026-05-11. Zero real uses yet.
- Eligible for persona-promotion review after 3+ real uses AND 2+ instances of needing material rework.
- Likely **not** a promotion candidate even at usage threshold — initialization is a one-shot task per repo, low repeat volume; better suited as a tightly-scoped playbook than a long-lived persona.
