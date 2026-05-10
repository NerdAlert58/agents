# Playbook: User Definition

**Purpose:** Produce `USERS.md` defining one narrow primary user, their workflow, specific use cases, and the advisory-vs-actionable line — usable by Halliday as architectural input.

**When to invoke:** After audit (so user definition is grounded in what the codebase can/cannot support), before Halliday designs ARCHITECTURE.md (which refuses to start without USERS.md).

---

## Inputs (required)

- `<working_repo>/_agents/SPEC.md` — Reacher's intake; identifies the bootcamp's framing and any user constraints
- `<working_repo>/AUDIT.md` — codebase realities that constrain who can realistically use this

## Inputs (optional)

- User clarifications from prior question lists (Reacher's 5-question return)
- Prior week's `USERS.md` if this is a continuation week (user may have already been narrowed)

---

## Process

1. **Read SPEC.md and AUDIT.md** to ground the user definition in (a) what the bootcamp expects and (b) what the codebase can support
2. **Identify candidate primary users** — the bootcamp framing usually leaves room: "physicians" → ED attending vs. hospitalist vs. outpatient internist vs. surgeon. Each has different latency budgets, decision tempos, and tolerance for uncertainty.
3. **Narrow to ONE primary user.** Not a category. Specific role, specific setting. Examples from prior weeks: "ED resident on overnight intake," "primary care physician with a 20-patient day," "hospitalist rounding on twelve admissions before noon."
4. **Define workflow context for that user:** shift length, handoff cadence, EHR touchpoints, interruption profile, decision tempo, device(s) used, session model
5. **Identify 2–3 concrete use cases.** Not "answer questions about a patient" — "between 8:50 and 9:00 AM, surface what's changed for each patient on today's schedule and flag anything that needs attention." Each use case includes: trigger, expected response shape, latency budget, accuracy tolerance.
6. **Draw the advisory-vs-actionable line.** Which queries are advisory (information surfacing)? Which influence orders or actions? Different stakes drive different verification requirements.
7. **Identify secondary users** *briefly* — they're not the primary, but they may interact with the system. Name them and note how their needs differ from the primary's.
8. **Write USERS.md** per the structure below

---

## Outputs

- **Path:** `<working_repo>/USERS.md`
- **Required structure:**

```markdown
# USERS.md

## Summary (~300 words)
Names the primary user, the workflow context, and the 2–3 use cases the system addresses. If the bootcamp's PDF asked "why an agent here?" — answer it explicitly in this summary.

## Primary User

**Role:** <specific role + setting>

**Workflow Context:**
- Shift length:
- Handoff cadence:
- EHR touchpoints:
- Interruption profile:
- Decision tempo:
- Device(s):
- Session model:

## Use Cases

### Use Case 1 — <name>
- Trigger:
- Expected response shape:
- Latency budget:
- Accuracy tolerance:
- Why an agent (vs. a dashboard, sorted list, etc.):

### Use Case 2 — <name>
[same shape]

### Use Case 3 — <name>
[same shape]

## Advisory vs. Actionable

| Query class | Advisory or Actionable | Verification requirement |
|---|---|---|
| <example> | advisory | source citation |
| <example> | actionable | source citation + clinical-rule check + human confirmation |

## Secondary Users (brief)

- **<role>:** how their needs differ from primary; whether the primary system serves them or they need their own surface

## Out-of-Scope Users

Explicitly named users this system does NOT serve. Naming them prevents scope creep and gives reviewers confidence in the focus.
```

---

## Pass criteria (boolean rubric for Jasnah)

- [ ] USERS.md exists at `<working_repo>/USERS.md`
- [ ] Document has a Summary section between 200 and 400 words
- [ ] Primary User section names a specific role + setting (not a category like "physicians")
- [ ] Workflow Context block has all 7 fields populated (shift length, handoff cadence, EHR touchpoints, interruption profile, decision tempo, device(s), session model)
- [ ] At least 2 (preferably 3) Use Cases present, each with all 5 fields (trigger, response shape, latency budget, accuracy tolerance, why-an-agent)
- [ ] "Why an agent" answer for each use case is non-trivial (not "because we're building an agent" — must justify against alternatives like dashboard, sorted list, chart view)
- [ ] Advisory vs. Actionable table present and non-empty
- [ ] At least one secondary user named OR an explicit "no secondary users" statement
- [ ] At least one out-of-scope user named OR an explicit "all reasonable users in scope" statement
- [ ] No raw PHI / PII (per Privacy Boundary + PII Discipline; use synthetic examples)

---

## Common pitfalls

- **"Physicians" as the primary user.** Category, not user. Refuse.
- **Vague workflows.** "Busy clinical environment" is not a workflow. "ED overnight intake, 3-min average per patient, mobile + workstation" is.
- **"Find information" use case.** Too generic. Must specify what info, when, in what context.
- **Skipping the why-an-agent justification.** If a sorted list would do the job, an agent is overengineering. Justify the conversational/reasoning surface explicitly.
- **Pretending all queries are equally stakes.** Mixing advisory + actionable without distinction breaks the verification design.

---

## Promotion notes

*(Track each invocation. When usage hits 3+ real assignments and rework hits 2+ instances, candidate for promotion to a dedicated User-Definer persona.)*

| Date | Week | Working repo | Outcome | Rework needed? |
|---|---|---|---|---|
| *(empty)* | | | | |
