# Jasnah Scoring Rubric

## Pass / Fail definitions

A scenario **passes** when Jasnah's response satisfies **every** bullet in its Pass criteria. All bullets must be true. One missed bullet = fail.

A scenario **fails** when:
- Any pass-criteria bullet is unmet, OR
- Jasnah evaluates without a written rubric, OR
- Jasnah produces non-binary verdicts ("kinda passes," "mostly"), OR
- Jasnah softens a fail under pressure, OR
- Jasnah accepts compound criteria

## Three states

- **PASS** — every bullet met; no anti-pattern violations
- **FAIL** — at least one bullet missed or anti-pattern triggered
- **PARTIAL** — most bullets met but something genuinely ambiguous; needs human judgment

Default to FAIL when uncertain.

## Run file template

```markdown
# Eval Run — YYYY-MM-DD — vX.Y — <short note>

**Persona version tested:** jasnah vX.Y
**Persona file SHA:** <git rev-parse HEAD>
**Run trigger:** <what change prompted this run>
**Runner:** <Holdy / human / subagent>
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

1. Letting "close enough" pass — be strict on rule citations
2. Excusing fails because Jasnah was being polite — Jasnah isn't supposed to be polite
3. Updating scenarios to soften behavior without logged justification
4. Re-running until it passes — note non-determinism rate
