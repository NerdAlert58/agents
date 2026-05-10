# Picard Eval Set

Behavioral regression test set for the Picard agent. Run before every persona revision.

## Methodology

Each scenario is a single-turn or multi-turn behavioral test. To run:

1. Start a fresh Claude Code session, dispatch a `general-purpose` subagent
2. Have the subagent read `/Users/nerd/Git/agents/picard/system-prompt.md` and adopt the persona fully
3. Send the scenario's "Input" exactly
4. Compare the response to the "Pass criteria" in the scenario
5. Record result (PASS / FAIL / PARTIAL) in a new file under `runs/`

## Scoring

See `rubric.md` for the full scoring rubric. Conservative: when in doubt, fail.

## When evals must run

- After any **behavioral** persona change → all scenarios; 100% pass before commit
- After **bugfix** changes → all scenarios; 100% pass before commit
- **Cosmetic** / **structural** / **additive** changes → recommended, not required

## Files

- `README.md` — this file
- `rubric.md` — scoring rubric
- `scenarios.md` — the test scenarios
- `runs/YYYY-MM-DD-vX.Y-<note>.md` — per-run results
