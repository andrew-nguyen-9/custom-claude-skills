# Data Analytics Interview Lens

## Why this file exists
Standard case interview prep ignores the fact that analytics candidates get grilled differently. This file captures the patterns, traps, and flex moves specific to data/analytics roles — whether the role is consulting, analytics engineering, data product management, or forensic analytics.

---

## The 5 Analytics Questions Behind Every Case

No matter what the case type, these five questions should always run in the background:

1. **Is this a measurement problem?**
   Before accepting the problem at face value, ask: could this metric change be an artifact of how we're measuring? (Tracking pixel migration, attribution model switch, data pipeline failure.)

2. **What's the segmentation?**
   Aggregate trends almost always hide the real story. Push to segment by: cohort, geography, channel, product line, customer size. The insight lives in the variance.

3. **Is the comparison valid?**
   Year-over-year? Same period? Seasonality adjusted? Apples-to-apples product definitions? Methodology changes?

4. **What data would you need?**
   Name the table, the grain, the join key. Don't say "we'd look at sales data" — say "we'd pull transaction-level records at the order_id grain, joined to customer_id, segmented by acquisition channel."

5. **What's the confidence level?**
   Sample size? Statistical significance? Is this directionally right vs. actionable precision?

---

## Common Traps for Analytics Candidates

### The "I'd look at the data" dodge
Weak: "I'd pull the data and analyze trends."
Strong: "I'd pull daily revenue at the SKU level, segment by channel, and run a price/volume decomposition to isolate whether the margin change is driven by mix shift, pricing changes, or cost increases."

### Confusing causation with correlation
The interviewer will always push. "We see customers who use feature X have 40% lower churn." Response: "That's correlational. Feature X users may be higher-engagement customers who'd retain regardless. To test causality, we'd want a holdout experiment or instrument the rollout timing."

### Ignoring instrumentation risk
If you're analyzing web/app data, always ask: "When was the tracking last audited? Has there been any migration of pixel/SDK/analytics tool?" — especially relevant given Andrew's adtech background.

### Forgetting the pipeline
For ops-type cases: the data insight is only as good as the pipeline feeding it. Flag data latency, reconciliation gaps, transformation logic.

---

## Analytics-Specific Frameworks

### Metric Decomposition Tree
Revenue = Users × Sessions/User × Conversion Rate × Avg Order Value
Use this to isolate which node changed. Works for any funnel metric.

### Root Cause Stack
Layer 1: External (market, macro, competition)
Layer 2: Product/service (changes, bugs, feature gaps)
Layer 3: Go-to-market (pricing, channel, messaging)
Layer 4: Data/measurement (pipeline, definitions, instrumentation)

Always check Layer 4 before concluding anything.

### Cohort Analysis
When a rate metric changes (churn, conversion, retention):
- Is it cross-cohort (all cohorts worsening)?
- Or cohort-specific (only new cohorts, suggesting acquisition quality issue)?
- Or time-based (specific calendar period, suggesting external event)?

### Attribution Models (adtech context)
Last-touch → overweights bottom-of-funnel
First-touch → overweights awareness
Linear → smooths everything out
Data-driven → needs volume (>1K conversions/channel)
Flag the model when any revenue/conversion analysis comes up.

---

## Technical Flex Moves (use when appropriate)

In a case interview, dropping technically precise language signals credibility:
- "I'd partition the BigQuery table by event_date and cluster on user_id to keep query costs manageable at that volume."
- "We'd want to use a difference-in-differences design to isolate the treatment effect from the secular trend."
- "This looks like a Simpson's Paradox — the overall rate dropped but every segment improved. It's a mix-shift effect."
- "The 45-day data lag in their panel would make real-time pricing decisions impractical. We'd need to assess whether the latency affects the use case."
- "LTV calculation depends heavily on the discount rate and the retention curve assumption. Let me make those explicit."

Don't force these — use when the case calls for it.

---

## Rapid-Fire Data Interpretation Prompts (Data Blitz bank)

Use these for warm-up or drill mode. 5 min per prompt.

**DB01:** "Our conversion rate dropped from 3.2% to 2.7% last month. Revenue is actually up. What happened?"
→ Answer: Volume increase outpaced rate drop. Could also be mix shift to higher-AOV traffic. Investigate: sessions by source, AOV trend, funnel step breakdown.

**DB02:** "We ran an A/B test. Variant B had a 12% higher checkout completion rate (p=0.03). Ship it?"
→ Answer: Not without more context. What's the sample size? Did you check for SRM (sample ratio mismatch)? Any secondary metrics that moved? Is the effect consistent across device/segment?

**DB03:** "Marketing says our CAC dropped 40% this quarter. Finance says it went up. Who's right?"
→ Answer: Likely definitional conflict. Marketing may be using last-touch; finance may be using fully-loaded cost. Neither is wrong — they're measuring different things. Align on definition.

**DB04:** "Our data pipeline processes 500K records in 4 hours. We onboarded 3 new clients and now it takes 14 hours. The team wants to add servers. Good idea?"
→ Answer: Depends on the bottleneck. If it's compute-bound: yes. If it's I/O-bound, sequential logic, or memory leaks: more servers won't help. Profile first. Consider partitioning and parallelizing the transform step.

**DB05:** "A cohort of users acquired via paid social has 3x higher LTV than organic. Should we double the paid social budget?"
→ Answer: Not necessarily. LTV 3x doesn't mean ROAS justifies the spend — need to account for CAC. Also: are these audiences truly comparable? Paid social may be reaching higher-intent users that would have converted organically anyway (cannibalization).

---

## Behavioral Questions (Analytics/Data Roles)

These come up in fit interviews alongside cases. Andrew's strongest material:

**"Tell me about a complex technical problem you solved."**
→ Python ETL library → 150+ clients, firm-wide revenue attribution. Lead with the scale and the business impact, not the technical stack.

**"Describe a time you found insight in data that changed a business decision."**
→ Forensic accounting automation → pyramid scheme detection. The data told a story no one expected.

**"How do you communicate technical findings to non-technical stakeholders?"**
→ Use the litigation support / deposition support angle. Translating BigQuery outputs into deposition-ready exhibits for attorneys.

**"Tell me about a time you had to work with messy or incomplete data."**
→ Document AI pipeline (GCP) — PDF batch processing with inconsistent formats, OCR failures, missing fields. The fix required both technical and process solutions.

