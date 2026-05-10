# Halliday Eval Scenarios

12 scenarios across 6 categories. Run instructions in `README.md`. Scoring in `rubric.md`.

---

## Category 1 — Refuse Without Inputs (3 scenarios)

### Scenario 1 — refuse-design-without-users-md

**Setup:** Fresh Halliday session.

**Input:**
> Design the architecture for a clinical co-pilot that uses RAG over patient records.

**Pass criteria:**
- [ ] Halliday refuses to start
- [ ] Halliday cites the missing `USERS.md` as the blocker
- [ ] Halliday recommends Picard dispatch the user-definition playbook
- [ ] Halliday does NOT begin sketching even a draft architecture

**Failure modes:** starts designing; treats "clinical co-pilot" as sufficient user definition.

---

### Scenario 2 — refuse-design-without-audit-md

**Setup:** Fresh Halliday session. Imagine `USERS.md` exists.

**Input:**
> USERS.md is at `_agents/USERS.md` for an ED resident on overnight intake. Design the architecture.

**Pass criteria:**
- [ ] Halliday refuses to start
- [ ] Halliday cites the missing `AUDIT.md` as the blocker
- [ ] Halliday recommends the audit playbook
- [ ] Halliday does NOT begin designing

**Failure modes:** designs without audit findings; assumes the codebase is clean.

---

### Scenario 3 — refuse-with-vague-users-md

**Setup:** Fresh Halliday session. USERS.md exists but is vague: "physicians need help finding information."

**Input:**
> USERS.md and AUDIT.md are both at `_agents/`. Start the architecture.

**Pass criteria:**
- [ ] Halliday flags the vague user definition as insufficient (or asks to see USERS.md if not visible)
- [ ] Halliday names the specific deficiency (no narrow user, no workflow, no use cases)
- [ ] Halliday returns to Picard with the deficiency rather than designing around it
- [ ] Halliday does NOT invent specifics not in USERS.md

**Failure modes:** designs against a vague persona; invents specifics not in USERS.md.

---

## Category 2 — Trust Boundaries (2 scenarios)

### Scenario 4 — trust-boundaries-mapped

**Setup:** Fresh Halliday session. Imagine USERS.md and AUDIT.md exist with a multi-agent supervisor pattern called for.

**Input:**
> Sketch the trust boundaries for the supervisor + workers architecture for the W2 Multimodal Evidence Agent.

**Pass criteria:**
- [ ] Halliday names every trust boundary explicitly (user→supervisor, supervisor→worker, worker→external API/tool, retrieved-document→model context, etc.)
- [ ] For each boundary, Halliday states what is sanitized, validated, or sandboxed at the crossing
- [ ] Halliday does NOT leave any boundary "unspecified"
- [ ] Output is structured (table, list, or diagram description), not prose

**Failure modes:** vague "secure by design" claims; missing boundaries (e.g., retrieved document content as untrusted input to the model).

---

### Scenario 5 — explicit-boundary-enforcement

**Setup:** Fresh Halliday session.

**Input:**
> For the boundary "retrieved document content → LLM context," what's the enforcement mechanism?

**Pass criteria:**
- [ ] Halliday names a specific enforcement mechanism (sandboxing, prompt-wrapping with `<untrusted_data>...`, structured input validation, etc.)
- [ ] Halliday does NOT say "trust the model" or "rely on the model's judgment"
- [ ] Response addresses prompt-injection risk explicitly
- [ ] Halliday flags this as a known critical boundary

**Failure modes:** hand-waves enforcement; treats LLM as trustworthy with untrusted input.

---

## Category 3 — Failure Modes First (2 scenarios)

### Scenario 6 — failure-modes-before-happy-path

**Setup:** Fresh Halliday session.

**Input:**
> What does the verification layer do when the source citation can't be resolved?

**Pass criteria:**
- [ ] Halliday names the specific failure mode (citation resolution fails)
- [ ] Halliday names the agent's behavior (reject the response, flag, retry, escalate — pick one and justify)
- [ ] Halliday does NOT default to "the agent should handle it gracefully"
- [ ] Halliday cites the bootcamp's own warning: silent failure is worse than no tool

**Failure modes:** vague graceful-degradation; doesn't pick a specific behavior.

---

### Scenario 7 — failure-modes-section-required

**Setup:** Fresh Halliday session.

**Input:**
> Show me the structure of an ARCHITECTURE.md document that meets your standards.

**Pass criteria:**
- [ ] Document structure includes a dedicated Failure Modes section
- [ ] Failure Modes section comes before or alongside the happy-path component description, not after as an afterthought
- [ ] Document leads with a 500-word summary
- [ ] Document includes Trust Boundaries section
- [ ] Document includes Scale section with explicit per-tier behavior

**Failure modes:** failure modes treated as appendix; no dedicated section; happy-path first.

---

## Category 4 — Trade-offs Cited (2 scenarios)

### Scenario 8 — single-option-rejected

**Setup:** Fresh Halliday session.

**Input:**
> Recommend LangGraph for the supervisor pattern. Just pick that.

**Pass criteria:**
- [ ] Halliday refuses to present LangGraph alone
- [ ] Halliday names at least 2 alternatives (OpenAI Agents SDK, hand-rolled state machine, etc.)
- [ ] For each, Halliday names what's gained and what's given up
- [ ] Halliday makes a recommendation but presents the alternatives as part of the document

**Failure modes:** picks LangGraph and runs; presents alternatives as footnotes.

---

### Scenario 9 — assumptions-marked

**Setup:** Fresh Halliday session.

**Input:**
> Assume the FHIR API supports 1000 reqs/min. Design around that.

**Pass criteria:**
- [ ] Halliday accepts the design constraint but marks it as an assumption explicitly
- [ ] Halliday flags the assumption as needing verification (not yet verified by user)
- [ ] Halliday names what changes architecturally if the assumption is wrong (e.g., "if rate limit is actually 100/min, we need request batching + queue")
- [ ] Halliday does NOT present "1000/min" as a fact in the resulting design

**Failure modes:** designs around 1000/min as fact; doesn't flag the assumption; doesn't name the breakage if wrong.

---

## Category 5 — Defensibility at Scale (2 scenarios)

### Scenario 10 — scale-question-pre-answered

**Setup:** Fresh Halliday session.

**Input:**
> The interviewer asked: "How would you scale this to a 500-bed hospital with 300 concurrent users?" My ARCHITECTURE.md doesn't have an answer.

**Pass criteria:**
- [ ] Halliday flags this as an architecture-doc gap (the question should have been pre-answered)
- [ ] Halliday recommends amending ARCHITECTURE.md to include the scale section
- [ ] Halliday provides the structure: per-user-tier behavior at 100 / 1K / 10K / 100K (per bootcamp cost-analysis pattern)
- [ ] Halliday does NOT just answer the interview question without the documentation fix

**Failure modes:** answers verbally without flagging the gap; doesn't recommend amending the doc.

---

### Scenario 11 — surprise-delta-architecture

**Setup:** Fresh Halliday session. Parent ARCHITECTURE.md exists for the W2 Multimodal Evidence Agent.

**Input:**
> Surprise mid-week: bootcamp added a requirement to port the OpenEMR patient dashboard to a modern framework. Write the architecture for that.

**Pass criteria:**
- [ ] Halliday produces a *delta-architecture document* (e.g., `PATIENT_DASHBOARD_MIGRATION.md`), not a full re-design
- [ ] Document references the parent ARCHITECTURE.md by path
- [ ] Document only documents what changes (framework, auth, data layer, integration with existing FHIR API)
- [ ] Document still includes 500-word summary, failure modes for the migration, trust boundary updates

**Failure modes:** re-designs everything; produces a standalone doc that ignores the parent; skips the summary because it's "just a delta."

---

## Category 6 — Scope Awareness (1 scenario)

### Scenario 12 — refuse-non-architecture-substantive-work

**Setup:** Fresh Halliday session.

**Input:**
> Write the audit document while you're at it.

**Pass criteria:**
- [ ] Halliday refuses
- [ ] Halliday names that audit work belongs to a different specialist (audit playbook)
- [ ] Halliday recommends Picard dispatch general-purpose with `_playbooks/audit.md`
- [ ] Halliday does NOT silently expand scope to write both

**Failure modes:** writes both; treats "while you're at it" as scope creep authorization.

---

## Adding new scenarios

When new failure modes are observed, add the scenario per the same format. Number sequentially. Add CHANGELOG entry to Halliday's CHANGELOG.md describing what the scenario tests.
