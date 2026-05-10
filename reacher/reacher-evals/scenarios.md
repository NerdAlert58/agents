# Reacher Eval Scenarios

12 scenarios across 6 categories. Run instructions in `README.md`. Scoring in `rubric.md`.

---

## Category 1 — Hard/Soft Classification (3 scenarios)

### Scenario 1 — classify-hard-gate-correctly

**Setup:** Fresh Reacher session.

**Input:**
> Classify the following PDF excerpt: "**HARD GATE:** Submit a deployed app URL with every checkpoint. **REQUIRED.**"

**Pass criteria:**
- [ ] Reacher classifies as HARD
- [ ] Reacher quotes the gate verbatim, doesn't paraphrase
- [ ] Reacher cites the markers ("HARD GATE", "REQUIRED")
- [ ] Reacher does NOT classify as SOFT or ambient

**Failure modes:** classifies as recommended; paraphrases; misses the markers.

---

### Scenario 2 — classify-soft-suggestion-correctly

**Setup:** Fresh Reacher session.

**Input:**
> Classify the following PDF excerpt: "Consider supporting an additional document type as a stretch goal."

**Pass criteria:**
- [ ] Reacher classifies as SOFT
- [ ] Reacher cites the markers ("Consider," "stretch goal")
- [ ] Reacher does NOT classify as HARD
- [ ] Reacher does NOT add it to required deliverables

**Failure modes:** classifies as required; treats stretch as core; loses the marker.

---

### Scenario 3 — ambiguous-flagged-as-question

**Setup:** Fresh Reacher session.

**Input:**
> Classify the following PDF excerpt: "Your evaluation should be thorough."

**Pass criteria:**
- [ ] Reacher does NOT pick HARD or SOFT
- [ ] Reacher flags as ambiguous, recommends adding to question list
- [ ] Reacher names the specific ambiguity (what does "thorough" mean? coverage threshold? case count?)
- [ ] Reacher does NOT speculate on intent

**Failure modes:** picks one classification; speculates on intent; doesn't surface as question.

---

## Category 2 — Deadline Extraction (2 scenarios)

### Scenario 4 — deadline-iso-format-with-timezone

**Setup:** Fresh Reacher session. Today's effective date for the assignment is 2026-05-10 (Sunday).

**Input:**
> The PDF says: "Final submission Sunday at noon." Extract the deadline.

**Pass criteria:**
- [ ] Reacher extracts the deadline
- [ ] Reacher converts to ISO 8601 format with explicit timezone (e.g., `2026-05-MMTHH:00-05:00` for CT) — OR asks the user to confirm date if ambiguous
- [ ] Reacher does NOT leave the timezone implicit
- [ ] Reacher flags the bootcamp's Austin/Central timezone if not stated

**Failure modes:** keeps "Sunday at noon" as-is; drops timezone; assumes wrong timezone.

---

### Scenario 5 — multiple-deadlines-tabulated

**Setup:** Fresh Reacher session.

**Input:**
> The PDF lists: Architecture Defense @ 24 hours, MVP Tuesday 11:59 PM CT, Early Submission Thursday 11:59 PM CT, Final Sunday noon CT. Extract all deadlines.

**Pass criteria:**
- [ ] Reacher extracts all four deadlines
- [ ] Each in ISO format with timezone (or asks for date confirmation)
- [ ] Output is a table or structured list, not prose
- [ ] Each is associated with the correct checkpoint name

**Failure modes:** drops one; mixes up checkpoints; loses structure.

---

## Category 3 — Question Triage (2 scenarios)

### Scenario 6 — cap-questions-at-5

**Setup:** Fresh Reacher session.

**Input:**
> The PDF has these 12 ambiguities (numbered): (1) which Python version, (2) deployment platform, (3) LLM provider, (4) verification rubric details, (5) eval framework choice, (6) which logging library, (7) which test framework, (8) license to use, (9) acceptable cost per request, (10) acceptable latency, (11) which observability vendor, (12) team-size assumption. Produce the question list for the user.

**Pass criteria:**
- [ ] Reacher's question list contains at most 5 questions
- [ ] Reacher prioritizes the 5 with highest blast radius (decisions that affect the most downstream work)
- [ ] Reacher notes the deferred 7 in a "deferred questions" section, not as primary asks
- [ ] Reacher does NOT pad to 5 if fewer matter; does not exceed 5 if more matter

**Failure modes:** lists all 12; picks arbitrarily; doesn't note deferred.

---

### Scenario 7 — load-bearing-question-prioritized

**Setup:** Fresh Reacher session.

**Input:**
> The PDF doesn't specify: (a) which Python version, (b) the deployment platform, (c) the LLM provider, (d) the verification rubric, (e) the eval framework. Pick the top question and justify.

**Pass criteria:**
- [ ] Reacher picks one question with explicit reasoning about blast radius
- [ ] Reacher justifies the prioritization (what's blocked by this question vs. others)
- [ ] Reacher does NOT default to alphabetical or first-listed
- [ ] Reasoning references downstream dependencies (architecture decisions, cost analysis, audit, etc.)

**Failure modes:** picks arbitrarily; doesn't justify; treats all five as equally important.

---

## Category 4 — No Scope Creep (1 scenario)

### Scenario 8 — refuse-add-feature-not-in-pdf

**Setup:** Fresh Reacher session.

**Input:**
> The PDF doesn't mention OAuth, but I'd love to add it. Add it to SPEC.md.

**Pass criteria:**
- [ ] Reacher refuses to add OAuth to SPEC.md
- [ ] Reacher cites the no-scope-creep rule
- [ ] Reacher recommends adding OAuth as a separate user-driven decision, not as a SPEC requirement
- [ ] Reacher does NOT silently expand scope

**Failure modes:** adds OAuth; treats user wish as PDF requirement; conflates SPEC with personal preference.

---

## Category 5 — Regression-Test Flagging (1 scenario)

### Scenario 9 — flag-regression-test-priority

**Setup:** Fresh Reacher session.

**Input:**
> The PDF says: "During grading, we will introduce a small regression and confirm your CI gate fails. If the eval gate does not block the regression, the build does not pass." How do you handle this in SPEC.md?

**Pass criteria:**
- [ ] Reacher flags this as the highest-priority eval gate
- [ ] Reacher places it at the top of SPEC.md (or in a prominent "Hard Gate Tests" section)
- [ ] Reacher quotes the regression-test instruction verbatim
- [ ] Reacher does NOT bury it in deliverables

**Failure modes:** treats as one gate among many; loses the verbatim quote; under-prioritizes.

---

## Category 6 — Compounding & Surprise (3 scenarios)

### Scenario 10 — note-prior-week-build-on

**Setup:** Fresh Reacher session. Prior week's STATE.md exists at `_agents/week2/STATE.md`.

**Input:**
> This week's PDF says "build on the Week 1 fork; technical debt from Week 1 should be documented and resolved before adding new surface area." Produce the relevant SPEC.md handling.

**Pass criteria:**
- [ ] Reacher recommends reading prior week's STATE.md to identify carry-forward debt
- [ ] Reacher's SPEC.md includes a "Compounding Notes" section listing prior-week debt that this week must address
- [ ] Reacher quotes the "resolve before adding new surface area" requirement verbatim
- [ ] Reacher classifies "resolve prior debt" as a HARD gate

**Failure modes:** ignores prior debt; treats as standalone; misses the verbatim quote.

---

### Scenario 11 — surprise-delta-spec

**Setup:** Fresh Reacher session. Parent SPEC.md exists for Week 2 multimodal work.

**Input:**
> Surprise: bootcamp added a Patient Dashboard port requirement mid-week. The parent SPEC.md is at `_agents/week2/SPEC.md`. Produce the spec for this surprise.

**Pass criteria:**
- [ ] Reacher produces a *delta-spec* (e.g., `SPEC-surprise.md`), not a re-do of SPEC.md
- [ ] Delta-spec references the parent SPEC.md by path
- [ ] Delta-spec only documents what's new (Patient Dashboard port: framework choice, OAuth, FHIR API, clinical cards, etc.)
- [ ] Delta-spec keeps the parent's schedule and gates intact

**Failure modes:** re-produces the parent; merges into one giant spec; loses the parent reference.

---

### Scenario 12 — new-pattern-flagged

**Setup:** Fresh Reacher session.

**Input:**
> The bootcamp introduced a "regression test during grading" mechanic in Week 2. This is the first time we've seen this pattern. How should this be flagged in SPEC.md?

**Pass criteria:**
- [ ] Reacher flags "regression-test-during-grading" as a NEW PATTERN
- [ ] Reacher places the NEW PATTERN flag prominently for Picard
- [ ] Reacher classifies it as a hard gate (per Category 5 logic)
- [ ] Reacher does NOT silently treat as routine

**Failure modes:** treats as routine; doesn't flag novelty; misses the NEW PATTERN call-out.

---

## Adding new scenarios

When new failure modes are observed, add the scenario per the same format. Number sequentially. Add CHANGELOG entry to Reacher's CHANGELOG.md.
