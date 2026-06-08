# Data Analytics Case Archetypes

Deep-dive reference for analytics-specific case types. Load this when running DA-series cases or when user wants to drill technical depth.

---

## Archetype 1: KPI Decline Diagnosis

**The pattern:** A metric dropped. You have n minutes to diagnose.  
**What interviewers are testing:** Structured thinking before touching data, data intuition, hypothesis prioritization

**Strong candidate behavior:**
1. Clarifies the metric definition and data source before hypothesizing
2. States 3–4 MECE hypotheses (internal change, external change, data artifact, measurement change)
3. Prioritizes by: likelihood × impact × ease of verification
4. Asks for segmented data rather than aggregate
5. Forms a "so what" — not just root cause, but business implication

**Weak candidate behavior:**
- Immediately asks for more data without forming hypotheses
- Treats all hypotheses as equal priority
- Stops at finding the number ("volume dropped") without explaining why
- Doesn't consider data quality as a hypothesis

**Sample worked example (DA-01):**

Prompt: Add-to-cart rate dropped 22% WoW. Marketing blames checkout UI. Engineering says nothing changed.

Strong opening:
> "Before I pull data, let me form a few hypotheses. 22% is a large single-week drop, so I'd look for a step-change event. My hypotheses: (1) a product or UI change — even if engineering says nothing changed, worth verifying with deploys and feature flags; (2) a traffic mix shift — if paid traffic spiked, and paid converts lower, the aggregate rate falls even if the underlying product is fine; (3) a data pipeline issue — maybe the event tracking for add-to-cart broke; (4) an external factor — competitor promotion pulling traffic, or a device-specific bug. I'd start by segmenting the drop by device type and acquisition channel, because those segments would either confirm or eliminate mix shift as the cause."

Note what this does: forms hypotheses FIRST, prioritizes them, starts with the most differentiating segmentation.

---

## Archetype 2: Experiment Evaluation

**The pattern:** A test ran. Should we ship?  
**What interviewers are testing:** Statistical intuition, awareness of common A/B pitfalls, business judgment

**Key questions a strong candidate asks:**
- Was the randomization unit appropriate? (user vs session vs page)
- Was the sample size pre-determined, or did they stop when p < 0.05? (peeking problem)
- How long did the test run? (novelty effects, seasonal bias)
- Are there network effects or cannibalization risks between groups?
- What's the practical significance vs statistical significance?
- What guardrail metrics were tracked?
- Is the treatment effect consistent across segments, or driven by one cohort?

**Common pitfalls to call out:**
- p = 0.09 is not "almost significant" — it means you haven't ruled out chance
- Underpowered tests can't confirm the null; they can only fail to reject it
- Conversion rate alone isn't enough — check downstream metrics (LTV, returns, support tickets)

---

## Archetype 3: Forensic / Litigation Analytics

**The pattern:** Find anomalies in data that tell a legal or regulatory story.  
**What interviewers are testing:** Methodological rigor, skepticism, ability to withstand cross-examination of methodology

**Key principles:**
- Document everything before analyzing (preserve chain of custody reasoning)
- Steelman the opposing interpretation — if your finding can be explained benignly, say so and address it
- Quantify uncertainty — don't say "the data shows fraud," say "there are N transactions with pattern X that deviate >3σ from expected behavior"
- Know the limits of your data — absence of evidence ≠ evidence of absence

**Forensic red flags in financial data:**
- Round number clustering (Benford's Law violations)
- Timing patterns: transactions near period-end, fiscal year-end
- Velocity spikes: sudden increase in transaction frequency without volume explanation
- Structural breaks: change in distribution after a specific date
- Intercompany patterns: related-party transactions that offset in suspicious ways

**What to say when data is incomplete:**
> "The gaps are themselves informative. If records are missing only for a specific date range or counterparty, that absence is a finding, not just a limitation. I'd want to document what's missing and why, and assess whether a complete dataset could be compelled through discovery."

---

## Archetype 4: Data Quality Assessment

**Framework: SCOPE**
- **S**ource: Where did this data come from? System of record or derived?
- **C**onsistency: Do values agree across tables/time?
- **O**utliers: Are there impossible or implausible values? (negative ages, future dates)
- **P**atterns: Do distributions match expectations? (Benford, seasonality)
- **E**xhaustiveness: Are key segments, dates, or entities missing?

**Standard QA checks:**
- Row counts by date — look for gaps or spikes
- Null rates by column — especially for join keys
- Duplicate detection — same business event logged multiple times
- Referential integrity — all foreign keys have corresponding parent records
- Temporal sequencing — event_B should always follow event_A for same entity

---

## Archetype 5: Metrics Design

**What makes a good metric:**
- Measurable: you can actually compute it with existing data
- Actionable: a change in the metric should lead to a clear response
- Sensitive: it moves when something meaningful changes
- Resistant to gaming: can't be inflated without actually improving the thing you care about
- Aligned: tells the same story at user level and aggregate level

**Common metric design mistakes:**
- Using ratio metrics without tracking numerator and denominator separately
- North star metric too lagging to be actionable
- No guardrails — can't tell if you're improving one thing while breaking another
- Composite scores that are uninterpretable when they move

**Hierarchy:**
```
North Star (one): captures core value delivered to users
Primary (2-4): direct drivers of north star
Guardrails (2-3): things you must not break
Diagnostic (many): used when primary metrics move
```

---

## Archetype 6: SQL / Schema Reasoning

**How to answer SQL reasoning questions without writing full code:**
1. State the tables you'd use and why
2. Identify the grain (what does one row represent?)
3. Describe the join logic and any de-dup risk
4. Call out window function need if applicable
5. State edge cases: nulls, duplicates, timezone issues, late-arriving data

**BigQuery-specific gotchas to know:**
- `TIMESTAMP` vs `DATETIME` — BQ timestamps are UTC, datetimes are timezone-naive
- Array/Struct columns — can't do standard joins; need `UNNEST`
- Dot-notation columns with reserved characters need backtick escaping
- Partitioned tables: always filter on partition key or you'll full-scan
- `SAFE_OFFSET` for array access to avoid out-of-bounds errors

