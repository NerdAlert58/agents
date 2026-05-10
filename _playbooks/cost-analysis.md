# Playbook: AI Cost Analysis

**Purpose:** Produce `AI_COST_ANALYSIS.md` documenting per-session token cost, multi-tier projections (100 / 1K / 10K / 100K users), architectural-change notes per tier, and bottleneck analysis.

**When to invoke:** After ARCHITECTURE.md exists (so cost can be computed against actual model/tool choices), before Final submission deadline. Bootcamp explicitly requires this deliverable from Week 1 onward.

---

## Inputs (required)

- `<working_repo>/ARCHITECTURE.md` — names the model(s), tools, retrieval pattern, latency targets
- `<working_repo>/USERS.md` — primary user's session profile (frequency, length, query types)

## Inputs (optional)

- Observability traces from running the system, if any exist
- Prior week's `AI_COST_ANALYSIS.md` if costs are evolving across weeks

---

## Process

1. **Read ARCHITECTURE.md** to identify: which LLM provider/model(s), which tools, which retrieval pattern, output budget, caching strategy
2. **Read USERS.md** to identify: typical session length, queries-per-session, user count assumption
3. **Compute per-session cost** with itemized math, not a single number:
   - Input tokens per query × $/M-token-input × queries/session
   - Output tokens per query × $/M-token-output × queries/session
   - Tool/retrieval overhead (vector store reads, reranker calls, FHIR API calls, etc.)
   - Cache savings (if prompt caching used; estimate hit rate)
4. **Project at four user tiers:** 100, 1K, 10K, 100K. For each:
   - Total cost / month (cost-per-session × sessions-per-user-per-month × users)
   - Cost / user / month
   - Architectural changes required at this tier (e.g., dedicated retrieval cluster, request batching, model tier-down, regional sharding)
   - Bottleneck identification (which component breaks first at this scale)
5. **Identify the actual ceiling** — at what user count does the architecture genuinely break, requiring re-architecture rather than scaling?
6. **Bottleneck analysis** — three layers (egress to LLM provider, retrieval/storage, app-tier compute). Which is most expensive? Which scales worst?
7. **Sensitivity analysis** — what's the cost impact of: switching to a cheaper model? halving cache hit rate? doubling session length? Pick 3 levers; show the swing.
8. **Recommendations** — 2–3 cost-reduction levers ranked by impact / effort

---

## Outputs

- **Path:** `<working_repo>/AI_COST_ANALYSIS.md`
- **Required structure:**

```markdown
# AI_COST_ANALYSIS.md

## Summary (~300 words)
Headline numbers (per-user/mo at each tier), the binding constraint at scale, the recommended cost-reduction lever. Hospital-CFO-readable.

## Per-Session Cost (itemized)

| Component | Tokens / call | Calls / session | $/M-token | Subtotal / session |
|---|---|---|---|---|
| LLM input | | | | |
| LLM output | | | | |
| Tool calls | | | | |
| Cache savings | | | | (negative) |
| **Total** | | | | **$ X.YY / session** |

Assumptions documented inline (model version, average query shape, etc.).

## Multi-Tier Projections

| Tier | Users | Sessions / user / month | Total cost / month | $/user/month | Bottleneck | Architectural changes required |
|---|---|---|---|---|---|---|
| 100 | 100 | | | | | |
| 1K | 1,000 | | | | | |
| 10K | 10,000 | | | | | |
| 100K | 100,000 | | | | | |

## Bottleneck Analysis

Which layer hurts first at each tier. Specifically: egress costs vs. retrieval costs vs. app-tier compute.

## Sensitivity Analysis

| Lever | Cost impact | Cost without lever |
|---|---|---|
| Cheaper model (X → Y) | -Z% | $A → $B |
| Cache hit rate halved | +Z% | $A → $B |
| Session length doubled | +Z% | $A → $B |

## Recommendations

1. **<top recommendation>** — impact, effort, dependencies
2. **<second recommendation>** — impact, effort, dependencies
3. **<third recommendation>** — impact, effort, dependencies

## Assumptions Marked Explicitly

Anything not verified against real usage data is marked `[ASSUMPTION — needs verification]`. Halliday's rule applies: do not present assumptions as facts.
```

---

## Pass criteria (boolean rubric for Jasnah)

- [ ] AI_COST_ANALYSIS.md exists at `<working_repo>/AI_COST_ANALYSIS.md`
- [ ] Document has a Summary section between 200 and 400 words
- [ ] Per-Session Cost table is itemized (NOT a single number) with components, math shown, and a Total
- [ ] Multi-Tier Projections table contains exactly 4 rows: 100, 1K, 10K, 100K users
- [ ] Each tier row has all 6 columns populated (users, sessions/user/mo, total $/mo, $/user/mo, bottleneck, architectural changes)
- [ ] Bottleneck Analysis section identifies which layer hurts first per tier — not "infrastructure is the bottleneck" generic
- [ ] Sensitivity Analysis covers at least 3 levers with quantified impact
- [ ] Recommendations section ranks at least 3 items by impact/effort
- [ ] Every assumption is explicitly marked `[ASSUMPTION — needs verification]`
- [ ] Document does NOT use "cost-per-token × N users" as the only math (bootcamp explicitly warns against this)

---

## Common pitfalls

- **"Cost-per-token times N users" only.** The bootcamp PDF explicitly says this is not enough. You must consider architectural changes at each tier.
- **Single bottleneck assumption.** The bottleneck shifts as you scale. At 100, app compute is fine; at 100K, the egress cost dominates.
- **Not pricing in cache savings.** Stable system prompt + tool definitions can be cached; pricing without cache hits inflates the number meaninglessly.
- **Skipping the sensitivity analysis.** Without it, the numbers look like guesses; with it, the analysis becomes defensible against "what if X changed?"
- **Recommendations without rank.** A list of 5 ideas with no priority is theater. Rank by impact/effort.

---

## Promotion notes

*(Track each invocation. When usage hits 3+ real assignments and rework hits 2+ instances, candidate for promotion to a dedicated Cost-Analyst persona.)*

| Date | Week | Working repo | Outcome | Rework needed? |
|---|---|---|---|---|
| *(empty)* | | | | |
