# Playbook: Codebase Audit

**Purpose:** Produce `AUDIT.md` covering the 5 required categories (Security, Performance, Architecture, Data Quality, Compliance) from a forked codebase, leading with a 500-word summary, with severity and evidence per finding.

**When to invoke:** After Reacher produces `_agents/SPEC.md` (so the audit can ground itself in the week's hard gates), before Halliday designs `ARCHITECTURE.md` (which requires `AUDIT.md` as input).

---

## Inputs (required)

- `<working_repo>/_agents/SPEC.md` — Reacher's intake spec; identifies what the audit must demonstrate the codebase can/cannot support
- `<working_repo>/` — root of the forked codebase (typically the OpenEMR fork or equivalent)

## Inputs (optional)

- Prior week's `<working_repo>/AUDIT.md` — if this is a continuation week, prior findings inform what's been resolved vs. carried forward
- Prior week's `_agents/STATE.md` — for compounding context

---

## Process

1. **Read SPEC.md in full.** Identify the week's hard gates and what the codebase must support.
2. **Survey the codebase structure.** `find`, `tree`, or `ls -R` to get a top-level inventory. Identify entry points, primary modules, third-party deps, configuration surfaces.
3. **Run each of the 5 audit categories**, in order:
   - **Security** — authentication mechanisms, authorization model, PHI/PII handling, input validation, secrets management, prompt-injection surface (for LLM-augmented systems)
   - **Performance** — bottleneck identification, query patterns, caching opportunities, current latency profile if measurable
   - **Architecture** — code organization, module boundaries, integration points, where the AI/agent layer fits or could fit cleanly
   - **Data Quality** — completeness of data, consistency across records, missing field rates, duplicate records, staleness — these become agent failure modes
   - **Compliance & Regulatory** — HIPAA-relevant gaps (audit logging, data retention, breach notification), BAA implications of LLM provider choice, de-identification requirements
4. **For each finding**, capture: title, severity (Critical / High / Medium / Low / Info), file path with line range or commit SHA as evidence, recommended remediation
5. **Write the 500-word summary** at the top — must highlight the most impactful findings, not be a flat dump or a TOC. The summary is what graders read first; if they read only this, they should know what's broken and what's at stake.
6. **Compile findings by category** below the summary, in severity order within each category
7. **Cross-reference SPEC.md hard gates** — for each hard gate, note whether the audit found evidence the codebase supports it, blocks it, or is silent

---

## Outputs

- **Path:** `<working_repo>/AUDIT.md`
- **Required structure:**

```markdown
# AUDIT.md

## Summary (~500 words, leads document)
<key findings; what's at stake; load-bearing gaps; what would change in the AI integration plan>

## Category 1 — Security
### Finding S-001: <title>
- Severity: <Critical|High|Medium|Low|Info>
- Evidence: <file:line range or commit SHA>
- Recommendation: <action>

### Finding S-002: ...

## Category 2 — Performance
[same shape]

## Category 3 — Architecture
[same shape]

## Category 4 — Data Quality
[same shape]

## Category 5 — Compliance & Regulatory
[same shape]

## Hard Gate Coverage (cross-reference to SPEC.md)
| Hard Gate | Codebase Status | Audit Reference |
|---|---|---|
| <gate from SPEC> | supports / blocks / silent | F-### |
```

---

## Pass criteria (boolean rubric for Jasnah)

- [ ] AUDIT.md exists at `<working_repo>/AUDIT.md`
- [ ] Document leads with a Summary section between 400 and 600 words
- [ ] All 5 required categories present and non-empty: Security, Performance, Architecture, Data Quality, Compliance
- [ ] Every finding has: title, severity drawn from {Critical, High, Medium, Low, Info}, evidence pointer (file:line or commit SHA), recommendation
- [ ] Zero findings use the word "should" in their evidence section (use "does" / "does not" / "is" / "is not")
- [ ] Hard Gate Coverage table cross-references each hard gate from SPEC.md to an audit finding or explicit "silent"
- [ ] No raw PHI / PII appears anywhere in the document (per Privacy Boundary + PII Discipline; use synthetic examples or redacted)

---

## Common pitfalls

- **Skipping the summary or making it a TOC.** The 500-word summary is what's read first; it must carry decision-relevant content, not list section titles.
- **Generic findings.** "Security could be better" is not a finding. "Authentication uses MD5 (server.php:142); recommend bcrypt migration" is.
- **Missing evidence pointers.** Without file:line or commit SHA, the audit can't be defended in interview.
- **Letting "Compliance" be just HIPAA name-checking.** Compliance covers audit logging, data retention, breach notification, BAA implications of LLM provider, de-identification — the acronym is the entry point, not the content.
- **Ignoring SPEC.md.** Audit findings that don't connect to the week's hard gates are decorative.

---

## Promotion notes

*(Track each invocation here as the playbook is used. When usage hits 3+ real assignments and rework hits 2+ instances, this playbook is a promotion candidate to a dedicated Auditor persona.)*

| Date | Week | Working repo | Outcome | Rework needed? |
|---|---|---|---|---|
| *(empty — populate on first invocation)* | | | | |
