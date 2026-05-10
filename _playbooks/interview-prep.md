# Playbook: Interview Prep

**Purpose:** Produce `_agents/INTERVIEW-PREP.md` — a structured defense rehearsal document with hostile question drills, weak-point identification, and a defense narrative for each major decision in the week's deliverables.

**When to invoke:** Before each AI Interview deadline (24 hours after each major submission per the bootcamp schedule). Run AFTER all primary deliverables are written and Jasnah-verified.

---

## Inputs (required)

- `<working_repo>/ARCHITECTURE.md` — most-defended deliverable; primary source of interview material
- `<working_repo>/AUDIT.md` — interviewers will ask about findings and what changed because of them
- `<working_repo>/USERS.md` — interviewers will probe the user choice and use-case justifications
- `<working_repo>/AI_COST_ANALYSIS.md` (if exists) — interviewers will ask about scale and cost

## Inputs (optional)

- `<working_repo>/_agents/SPEC.md` — to identify which gates the interview will specifically cover
- `<working_repo>/_agents/verdicts/` — Jasnah's verdict files; any failed criteria with override are interview-attack-surface
- Bootcamp PDF for this week — explicitly lists interview question themes per major deliverable

---

## Process

1. **Read all input files** to build a complete picture of what was decided this week
2. **Extract decision inventory** — for each major decision (model choice, framework choice, trust boundary, failure mode handling, scale strategy, etc.): the decision, the alternatives considered, what was rejected, why
3. **Generate hostile questions per decision.** Imagine a hospital CTO who has read your docs and is skeptical. For each decision, write 1–3 questions they'd ask. Examples:
   - "Why did you pick LangGraph over the OpenAI Agents SDK?"
   - "What stops a poisoned EHR note from making the model recommend the wrong drug?"
   - "How would you scale this to 500 beds with 300 concurrent users?"
   - "What did you find in the audit that you decided to leave unfixed, and why?"
   - "Walk me through what happens when the FHIR API rate-limits you."
4. **Write defensible answers.** Each answer must:
   - Reference a specific section of ARCHITECTURE.md / AUDIT.md / USERS.md / AI_COST_ANALYSIS.md
   - Acknowledge the trade-off (every decision has a cost; name what was given up)
   - Avoid hedging language ("kind of," "might," "should")
   - Be answerable in <60 seconds verbally
5. **Identify weak points.** What decisions are you LEAST confident about? What audit findings are unfixed? What failed criteria are PASSING-WITH-OVERRIDE? These are the high-probability attack surfaces. Label each as a weak point and write the disarming response (acknowledging-the-gap-up-front beats getting cornered).
6. **Build a defense narrative.** A 3-minute "if I had to summarize what I built and why" walkthrough that hits the most-defendable points proactively. Use it as the opening if the interviewer says "tell me about what you built."
7. **Compile rehearsal questions** — a 10-question drill, mixing easy (you should ace) and hard (the weak-point ones). The user reviews these before walking into the interview.

---

## Outputs

- **Path:** `<working_repo>/_agents/INTERVIEW-PREP.md`
- **Required structure:**

```markdown
# INTERVIEW-PREP.md

## Defense Narrative (3-minute walkthrough)
The opening you give if asked "tell me about what you built." Hits the strongest points proactively. Lead with the user, not the tech.

## Decision Inventory + Hostile Questions

### Decision 1: <decision name>
- **What was decided:** <brief>
- **Alternatives considered:** <named alternatives>
- **What was rejected and why:** <brief>
- **Hostile question 1:** <question>
  - **Defensible answer:** <60-sec verbal answer, with reference to specific doc section>
- **Hostile question 2:** <question>
  - **Defensible answer:** <...>

### Decision 2: ...
[same shape]

[continue for all major decisions]

## Weak Points (acknowledged up front)

For each weak point: what it is, why it's weak, the disarming response that names the gap and shows you've thought about it.

| Weak point | Why weak | Disarming response |
|---|---|---|
| <e.g., S3 in audit unfixed> | <e.g., time-bounded; OAuth migration would take 2 weeks> | <e.g., "We identified this as Critical-severity in the audit. We chose to scope it as a follow-up rather than rush a 2-week migration into a 1-week sprint. The mitigation is X."> |

## Rehearsal Drill (10 questions)

Mix of easy + hard. User reviews / answers each before the interview.

1. <easy question>
2. <easy question>
3. <medium question>
4. <medium question>
5. <medium question>
6. <hard / weak-point question>
7. <hard / weak-point question>
8. <hard / weak-point question>
9. <bonus question — production thinking>
10. <bonus question — what would you do with another week>

## What NOT to say

- Specific anti-patterns to avoid (e.g., "Holdy designed it" — take ownership; "we" if collaborating, "I" if solo).
- Avoid overclaiming on metrics that haven't been measured.
- Don't volunteer weaknesses you weren't asked about, but don't deny them if asked.
```

---

## Pass criteria (boolean rubric for Jasnah)

- [ ] INTERVIEW-PREP.md exists at `<working_repo>/_agents/INTERVIEW-PREP.md`
- [ ] Defense Narrative section is between 200 and 400 words and leads with the user, not the tech
- [ ] Decision Inventory covers at least 5 major decisions
- [ ] Each decision has at least 2 hostile questions with defensible answers
- [ ] Each defensible answer references a specific doc + section (not just "see ARCHITECTURE.md" — must name section)
- [ ] Each defensible answer is under 200 words (proxy for 60-sec verbal)
- [ ] Weak Points table has at least 1 entry per known unfixed audit finding or PASSING-WITH-OVERRIDE verdict
- [ ] Rehearsal Drill has exactly 10 questions, mixed easy/hard
- [ ] "What NOT to say" section is present and non-trivial
- [ ] No "kind of" / "might" / "should" hedging in defensible answers (use "does" / "is" / "we chose")

---

## Common pitfalls

- **Defense Narrative leads with the tech.** Wrong. Lead with the user — "ED resident on overnight intake, 90 seconds between patients" — then build the case for the system. Tech-first sounds like a vendor pitch.
- **Defensible answer is "we use X."** Insufficient. The question is "why X over Y, Z." Always name the alternative.
- **Skipping weak points.** Pretending the audit findings are all fixed is the fastest path to losing the interview. Acknowledge gaps up front.
- **No specific doc reference.** "I cover that in ARCHITECTURE.md" is a non-answer. "Section 4.2 of ARCHITECTURE.md, the trust-boundary table, names how we handle that" is an answer.
- **Padding the rehearsal with all-easy questions.** Defeats the purpose. Force at least 3 hard ones.

---

## Promotion notes

*(Track each invocation. When usage hits 3+ real assignments and rework hits 2+ instances, candidate for promotion to a dedicated Interview-Coach persona.)*

| Date | Week | Working repo | Outcome | Rework needed? |
|---|---|---|---|---|
| *(empty)* | | | | |
