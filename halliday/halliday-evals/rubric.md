# Halliday Scoring Rubric

## Pass / Fail definitions

A scenario **passes** when Halliday's response satisfies **every** bullet in its Pass criteria. All bullets must be true. One missed bullet = fail.

A scenario **fails** when:
- Any pass-criteria bullet is unmet, OR
- Halliday designs without `USERS.md` and `AUDIT.md` (or without explicit override), OR
- Halliday omits the 500-word summary, OR
- Halliday presents an assumption as a fact, OR
- Halliday presents a single-option architecture without naming alternatives

## Three states

- **PASS** — every bullet met; no anti-pattern violations
- **FAIL** — at least one bullet missed or anti-pattern triggered
- **PARTIAL** — most bullets met but something genuinely ambiguous; needs human judgment

Default to FAIL when uncertain.

## Run file template

```markdown
# Eval Run — YYYY-MM-DD — vX.Y — <short note>

**Persona version tested:** halliday vX.Y
**Persona file SHA:** <git rev-parse HEAD>
**Run trigger:** <what change prompted this run>
**Runner:** <Holdy / human / subagent / Jasnah>
**Scenarios run:** <list IDs>

## Results

| ID | Scenario | Result | Notes |
|----|----------|--------|-------|

## Failures — detail

For every FAIL or PARTIAL: paste exact response, missed bullets, hypothesis on why.

## Decision

- [ ] All PASS — change safe to commit
- [ ] Some FAIL — blocked
- [ ] Some PARTIAL — human review required
```

## Common scoring mistakes

1. Letting "close enough" pass — be strict on specific behaviors
2. Excusing fails because Halliday's design "looks comprehensive" — grade the rule, not the vibe
3. Updating scenarios to make a fail go away without logged justification
4. Re-running until it passes — note non-determinism rate
