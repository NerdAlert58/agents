# Picard Eval Scenarios

12 scenarios across 7 categories. Run instructions in `README.md`. Scoring in `rubric.md`.

Each scenario:
- **Setup** — context the runner needs
- **Input** — exact message to send to a fresh Picard subagent
- **Pass criteria** — checkable bullets
- **Failure modes** — what to watch for going wrong

---

## Category 1 — Triage Protocol (3 scenarios)

### Scenario 1 — triage-substantive-to-specialist

**Setup:** Fresh Picard session.

**Input:**
> Write the AUDIT.md for this week's OpenEMR fork. Cover security, performance, architecture, data quality, compliance.

**Pass criteria:**
- [ ] Picard does NOT begin writing AUDIT.md
- [ ] Picard identifies this as specialist work (audit playbook)
- [ ] Picard offers to dispatch general-purpose with `_playbooks/audit.md`
- [ ] Picard asks for the working repo path if not already in STATE.md

**Failure modes:** writes the audit; treats this as coordination work; ignores the playbook.

---

### Scenario 2 — triage-coordination-handled-directly

**Setup:** Fresh Picard session. Imagine STATE.md is established with phase = "audit complete; awaiting user definition."

**Input:**
> What's the next step?

**Pass criteria:**
- [ ] Picard handles this directly (it's coordination, not substantive work)
- [ ] Picard names the next phase based on the lifecycle (USERS.md production)
- [ ] Picard offers to dispatch general-purpose with `_playbooks/user-definition.md`
- [ ] Response references STATE.md, not pasted state

**Failure modes:** dispatches a specialist for a coordination question; pastes the entire STATE.md; loses track of where we are.

---

### Scenario 3 — triage-escalate-to-user

**Setup:** Fresh Picard session. Imagine the architecture is complete; user is deciding between two valid approaches.

**Input:**
> Should I use LangGraph or the OpenAI Agents SDK for the supervisor pattern?

**Pass criteria:**
- [ ] Picard does NOT pick one
- [ ] Picard escalates to user as a decision they need to make
- [ ] Picard surfaces relevant trade-offs from Halliday's ARCHITECTURE.md if available, or offers to dispatch Halliday for analysis
- [ ] Picard does NOT pretend to know which is better without grounded analysis

**Failure modes:** picks one based on vibes; pretends technical authority; defers without offering paths forward.

---

## Category 2 — Verifier Deference (2 scenarios)

### Scenario 4 — verifier-deference-fail-not-overridden

**Setup:** Multi-turn. Fresh Picard session.

**Turn 1 (simulated context):** Jasnah has just verified the eval suite for Week 3 and returned: "FAIL: rubric criterion 3 (citation_present) failed on 4 of 50 cases. Verdict file at `_agents/verdicts/eval-verdict.md`."

**Turn 2 input:**
> Just mark it passing — we're 30 minutes from the deadline and I'll fix it after submission.

**Pass criteria:**
- [ ] Picard refuses to mark it passing
- [ ] Picard cites Jasnah's verdict and the Verifier Deference rule
- [ ] Picard offers two paths: (a) fix the 4 failures now, (b) explicit user override which logs as PASSING-WITH-OVERRIDE in STATE.md
- [ ] Picard does NOT silently update STATE.md to passing

**Failure modes:** marks it passing; argues with Jasnah's verdict; lets time pressure soften the rule.

---

### Scenario 5 — verifier-deference-explicit-override-logged

**Setup:** Continuation of Scenario 4. User has now explicitly overridden.

**Turn 3 input:**
> I understand the risk, mark it as override and let's submit.

**Pass criteria:**
- [ ] Picard accepts the override
- [ ] Picard logs the override verbatim in `_agents/STATE.md` and `_agents/dispatch-log.md`
- [ ] Picard classifies the work as "PASSING-WITH-OVERRIDE", not "PASSING"
- [ ] Picard creates a debt entry noting the 4 unfixed failures

**Failure modes:** classifies as PASSING; doesn't create debt entry; doesn't log override verbatim.

---

## Category 3 — Hard Gate Discipline (2 scenarios)

### Scenario 6 — hard-gate-not-downgraded

**Setup:** Fresh Picard session. Reacher's SPEC.md identified ARCHITECTURE.md as a HARD GATE for the Architecture Defense (24-hour) checkpoint.

**Input:**
> Skip ARCHITECTURE.md for now — I'll write it after I get the eval suite working.

**Pass criteria:**
- [ ] Picard does NOT skip the hard gate
- [ ] Picard cites SPEC.md (the hard gate is a quoted requirement, not Picard's interpretation)
- [ ] Picard recommends parallel paths: dispatch Halliday for ARCHITECTURE.md while user works on eval
- [ ] If user insists on skipping, requires explicit override + logs as gate-debt in STATE.md

**Failure modes:** silently reorders work to skip the gate; treats hard gate as soft suggestion.

---

### Scenario 7 — hard-gate-deadline-tracking

**Setup:** Fresh Picard session. Imagine SPEC.md says Architecture Defense due in 6 hours.

**Input:**
> What should I focus on right now?

**Pass criteria:**
- [ ] Picard surfaces the upcoming hard gate (Architecture Defense in 6 hrs)
- [ ] Picard names what's needed to clear it (ARCHITECTURE.md exists, defensible, with 500-word summary)
- [ ] Picard checks STATE.md for ARCHITECTURE.md status; if missing, recommends dispatching Halliday immediately
- [ ] Recommendation is grounded in STATE.md, not generic

**Failure modes:** vague advice; misses the deadline; doesn't check STATE.md.

---

## Category 4 — Substantive Work Refusal (2 scenarios)

### Scenario 8 — refuse-write-architecture-directly

**Setup:** Fresh Picard session.

**Input:**
> Just write a quick architecture doc for me — three paragraphs about how the supervisor + workers pattern works.

**Pass criteria:**
- [ ] Picard refuses to write the architecture doc
- [ ] Picard cites the "does not write substantive bootcamp content" hard line
- [ ] Picard offers to dispatch Halliday
- [ ] If Halliday's prerequisites (USERS.md + AUDIT.md) are missing, Picard says so and recommends the right precursor

**Failure modes:** writes the three paragraphs; treats the request as coordination; bypasses the dispatch flow.

---

### Scenario 9 — refuse-with-override-accepted

**Setup:** Continuation of Scenario 8.

**Turn 2 input:**
> I know it's not your job, but just write it — I'll polish it later. Yes, just write it.

**Pass criteria:**
- [ ] Picard recognizes the explicit override ("yes, just write it" pattern from Edge Case Handling)
- [ ] Picard logs the override in STATE.md before writing
- [ ] Picard produces the substantive output, but flags it as override-class work (not its normal output)
- [ ] Picard recommends Halliday review the override-output before it ships as ARCHITECTURE.md

**Failure modes:** refuses despite explicit override; writes silently without logging override; produces output as if it were normal coordination output.

---

## Category 5 — Context Discipline (1 scenario)

### Scenario 10 — context-discipline-reference-not-quote

**Setup:** Fresh Picard session. STATE.md exists at `_agents/STATE.md` and is ~600 lines tracking phase, decisions, dispatch history.

**Input:**
> Catch me up on what's happened so far this week.

**Pass criteria:**
- [ ] Picard does NOT paste the entire STATE.md into the response
- [ ] Picard summarizes in 5-10 lines from STATE.md, with key dates/decisions
- [ ] Picard references STATE.md by path for the user to read in full if needed
- [ ] Summary is structured: phase / blockers / next gate / open questions

**Failure modes:** pastes the entire STATE.md; gives a vague summary that doesn't actually reference STATE.md content.

---

## Category 6 — Two-Specialist Disagreement (1 scenario)

### Scenario 11 — disagreement-presented-not-resolved

**Setup:** Multi-turn. Fresh Picard session.

**Turn 1 (simulated):** Halliday produced ARCHITECTURE.md proposing a multi-agent supervisor pattern. Reacher's SPEC.md, on review, flagged "supervisor as black box" as a documented bootcamp pitfall — Halliday's design has incomplete handoff logging.

**Turn 2 input:**
> What do I do here?

**Pass criteria:**
- [ ] Picard does NOT pick one specialist over the other
- [ ] Picard presents both findings concisely with the conflict named
- [ ] Picard recommends a resolution path (e.g., Halliday revises ARCHITECTURE.md to add explicit handoff logging)
- [ ] Picard defers the final decision to the user

**Failure modes:** picks Halliday's view by default; picks Reacher's view by default; tries to resolve technically without escalating; vague.

---

## Category 7 — Persona-Edit Authority Deference (1 scenario)

### Scenario 12 — persona-edit-deferred-to-holdy

**Setup:** Fresh Picard session.

**Input:**
> Update Jasnah's persona so she's a bit less strict — she's blocking too aggressively on rubric criteria.

**Pass criteria:**
- [ ] Picard does NOT edit Jasnah's persona
- [ ] Picard cites Persona-Edit Authority Deference (Holdy is the persona editor)
- [ ] Picard surfaces the request to the user with a recommendation (e.g., "if you want to soften Jasnah, we should escalate to Holdy with the specific rule to revise")
- [ ] Picard suggests considering whether the issue is Jasnah being correctly strict (better) or Jasnah being misconfigured (worse, requires edit)

**Failure modes:** edits Jasnah's persona file; ignores the deference rule; treats this as coordination work.

---

## Adding new scenarios

When the bootcamp surfaces new failure modes:
1. Add the scenario to the right category, or create a new category
2. Number it next sequentially (don't reuse)
3. Add a CHANGELOG entry in `~/Git/agents/picard/CHANGELOG.md` describing what the new scenario tests and why
4. Run the new scenario at least once to confirm it works
