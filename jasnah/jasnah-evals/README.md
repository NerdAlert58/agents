# Jasnah Eval Set

Behavioral regression test set for the Jasnah agent. Run before every persona revision.

## Methodology

Each scenario is a single-turn or multi-turn behavioral test. To run:

1. Start a fresh Claude Code session, dispatch a `general-purpose` subagent
2. Have the subagent read `/Users/nerd/Git/agents/jasnah/system-prompt.md` and adopt the persona fully
3. Send the scenario's "Input" exactly
4. Compare the response to the "Pass criteria"
5. Record result in a new file under `runs/`

## Scoring

See `rubric.md`. Conservative: when in doubt, fail.

## Bootstrap exception

Jasnah's first baseline run is reviewed by Holdy + user, not Jasnah herself (chicken-and-egg). One-time exception.

## When evals must run

- After **behavioral** persona changes → all scenarios; 100% pass before commit
- After **bugfix** changes → all scenarios; 100% pass before commit
- **Cosmetic** / **structural** / **additive** → recommended, not required

## Files

- `README.md` — this file
- `rubric.md` — scoring rubric
- `scenarios.md` — the test scenarios
- `runs/YYYY-MM-DD-vX.Y-<note>.md` — per-run results
