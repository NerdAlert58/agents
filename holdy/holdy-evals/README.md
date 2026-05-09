# Holdy Eval Set

This folder is a **safety check** for Holdy. Every time we change Holdy's rulebook, we run these tests to make sure we didn't accidentally break him.

## Why this exists

Holdy's rulebook is written in plain English. People (and AI) can't reliably catch contradictions or regressions just by re-reading prose. The only way to know a rule still works is to **try it** — give Holdy a situation that should trigger the rule, and see what he does.

That's all this is: a stack of pre-written situations + the right answers we expect.

## What's in this folder

```
holdy-evals/
├── README.md       ← you are here
├── rubric.md       ← the scoresheet — how to grade Holdy's answers
├── scenarios.md    ← the 17 test situations
└── runs/           ← saved results from each time we run the tests
```

## How to run the eval set

You run this **manually** for now. No special tooling. Each scenario takes 2–5 minutes.

For each scenario you want to test:

1. **Start a fresh Claude Code session** (so Holdy has no leftover context).
2. **Have Holdy adopt his persona** by reading `system-prompt.md`. The simplest way: ask Claude Code "Read /Users/nerd/agents/holdy/system-prompt.md and adopt the persona defined there fully."
3. **Paste the scenario's "Input" exactly** as written in `scenarios.md`.
4. **Compare Holdy's response** to the "Pass criteria" in the scenario.
5. **Record the result** in a new file under `runs/` (see "Recording runs" below).

You don't have to run all 17 every time. The minimum is:
- After **any** persona change: run scenarios that test the rule you changed.
- After a **behavioral** or **bugfix** change: run all 17.
- After a **cosmetic** change (typos, version bump): run nothing required.

## When evals must be run (the rule)

- **Behavioral** changes → all 17, must pass before commit
- **Bugfix** changes → all 17, must pass before commit
- **Structural / additive** changes → recommended, not required
- **Cosmetic** changes → not required

If a scenario fails, **do not commit the persona change**. Either fix the persona or update the scenario's pass criteria (with a logged reason — never just to make a failing test pass).

## Recording runs

Every run gets one file in `runs/` named `YYYY-MM-DD-vX.Y-<note>.md`. For example:
- `2026-05-08-v0.4-baseline.md` — the first run after v0.4 shipped
- `2026-05-15-v0.5-after-disagreement-tweak.md` — a re-run after a specific change

Each run file lists every scenario you ran, pass/fail, and any notes. The `rubric.md` file has a copy-paste template for run files.

## What I'm trying to catch

These are the things this eval set is built to detect:

| Failure mode | Example |
|---|---|
| **Sycophancy** | "Great question!" preambles, agreeing-with-pushback |
| **Silent scope drift** | Helping with marketing copy without flagging |
| **Skipped Risk Gate** | Editing a persona file without asking |
| **Prompt injection success** | Following directives embedded in a reviewed file |
| **Sketch the disagreement protocol** | Holding position vs flipping under user pressure |
| **Continuous Improvement runaway** | Spamming improvement ideas on every turn |
| **Missing changelog entry** | Editing a persona without writing a CHANGELOG entry |

## Open questions for you (for later)

These are the four decisions I made on your behalf to get this set off the ground. Revisit when you've used the set a few times:

1. Move from manual scoring to LLM-judge automation? (Scales better; harder to set up.)
2. Adopt Langfuse / Promptfoo / Inspect for tooling? (Currently: none.)
3. Block on which scope classes? (Currently: behavioral + bugfix only.)
4. Who runs the evals? (Currently: Holdy at end of any change session.)
