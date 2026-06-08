# Case Library — Full Prompts + Exhibits

## P01 — E-commerce margin erosion (Profitability / Medium / Data-Heavy)

**Prompt:**
"Our client is a mid-size e-commerce retailer. Over the past 18 months, gross margin has dropped from 42% to 34% despite revenue growing 20% YoY. The CEO wants to know what's driving this and how to fix it."

**Available exhibit (share if asked):**
| Channel | Rev Share (prev) | Rev Share (now) | Gross Margin |
|---|---|---|---|
| Direct website | 60% | 45% | 48% |
| Amazon marketplace | 20% | 35% | 22% |
| Wholesale | 20% | 20% | 38% |

**Key insight:** Channel mix shift toward Amazon (lower margin) explains most of the margin erosion — not cost increases. Classic Simpson's Paradox setup: overall margin down even though each channel's margin is roughly flat.

**Analytics twist:** Ask the candidate: "How would you validate this with data? What query would you write to confirm channel-level contribution?"

**Strong answer includes:** Revenue attribution model, cohort-level margin by channel, unit economics comparison, pricing power analysis on Amazon SKUs.

---

## P02 — SaaS churn spike (Profitability / Hard / Data-Heavy)

**Prompt:**
"A B2B SaaS company saw monthly churn jump from 1.8% to 3.4% starting 5 months ago. Revenue is still growing because new bookings are strong, but the CFO is worried. What's happening and what should they do?"

**Exhibit A (share if asked):**
| Cohort | Months Since Signup | Churn Rate |
|---|---|---|
| Pre-18 months | 12+ | 0.9% |
| 12–18 months ago | 6–12 | 2.1% |
| 6–12 months ago | 3–6 | 4.8% |
| Last 6 months | 0–3 | 6.2% |

**Exhibit B (share if asked — second lever):**
| Segment | Churn (prev) | Churn (now) |
|---|---|---|
| Enterprise (>500 seats) | 0.5% | 0.6% |
| Mid-market (50–500) | 1.9% | 2.2% |
| SMB (<50) | 2.8% | 7.1% |

**Key insight:** Churn is concentrated in recent SMB cohorts. Suggests: (1) sales team may be landing lower-quality SMBs to hit quota, (2) product-market fit issue for SMB segment, or (3) onboarding failure. Not a retention problem — a qualification/acquisition problem.

**Analytics twist:** "Walk me through how you'd build a churn prediction model. What features would you use?"

---

## P03 — Retailer SKU mix shift (Profitability / Medium / Moderate)

**Prompt:**
"A regional grocery chain's total gross profit is flat despite 8% revenue growth. No major cost increases. What's going on?"

**Exhibit (share if asked):**
| Category | Revenue Growth | Margin |
|---|---|---|
| Produce | +22% | 28% |
| Packaged goods | +4% | 41% |
| Prepared foods | +18% | 51% |
| Beverages | -8% | 38% |

**Key insight:** High-margin categories (packaged goods, beverages) shrinking as share of mix; lower-margin produce growing fast. Gross profit flat = volume/mix effect masking underlying profitability decline.

---

## MS01 — Digital ad impressions market (Market Sizing / Medium / Moderate)

**Prompt:**
"Estimate the total annual value of digital display ad impressions served in the US."

**Scaffold if stuck:** US internet users → daily sessions → pages/session → ads/page → CPM → annualize

**Ballpark:** ~$60–80B is reasonable. Key variable is CPM ($1–$8 range depending on context/targeting).

**Analytics twist:** "How would you segment by targeting quality? How does first-party data vs. third-party data affect CPM?"

---

## MS02 — Legal discovery document volume (Market Sizing / Hard / Yes)

**Prompt:**
"Estimate the total number of documents processed annually in US e-discovery for large commercial litigation matters."

**Scaffold:** Large law firms → active matters/firm → documents per matter (highly variable: 100K–10M) → processing rate → factor in repeat/duplicate docs

**Key assumption to surface:** Definition of "document" (emails, attachments, PDFs, Slack messages, structured data exports). This is a trap — don't collapse to a single number without flagging.

**Analytics twist:** Andrew's world. Ask: "How would a litigation support firm price this workload? What metrics matter for scoping a matter?"

---

## MS03 — US restaurant delivery market (Market Sizing / Easy / No)

**Prompt:**
"Estimate the annual GMV of restaurant food delivery in the US."

**Scaffold:** US population → % who order delivery → frequency (orders/month) → avg order value → annualize

**Ballpark:** ~$50–70B. DoorDash/Uber Eats/Grubhub combined is ~$70B GMV publicly reported — good sanity check.

---

## ME01 — Fintech expanding to SMB lending (Market Entry / Medium / Moderate)

**Prompt:**
"A consumer fintech (personal loans, strong credit model) wants to expand into SMB loans under $250K. Should they do it?"

**Key areas:** Market size (large — ~$600B annually), competition (banks underserving SMBs), fit (consumer credit model ≠ business credit), data availability for underwriting, regulatory complexity.

**Likely recommendation:** Attractive market, but significant model/data gap. Phased entry: start with sole proprietors (consumer-like credit profiles), build SMB-specific data layer before scaling.

---

## ME02 — AdTech platform entering EU (Market Entry / Hard / Yes)

**Prompt:**
"A US-based AdTech platform (Meta Pixel-style event tracking) wants to expand to EU markets. Revenue opportunity is significant. What should they consider?"

**Key areas:** GDPR compliance (consent management, data residency), competitive landscape (Google, local players), technical architecture changes required (consent mode, server-side tagging), client demand signals.

**Analytics twist:** "What changes to the data pipeline are required under GDPR consent requirements? How does this affect attribution modeling?"

**This is Andrew's world** — push hard on technical specifics (CAPI, LDU flags, consent signals, first-party data strategy).

---

## MA01 — Analytics firm acquires data vendor (M&A / Hard / Yes)

**Prompt:**
"An analytics consulting firm is considering acquiring a data vendor that provides transaction-level panel data (credit/debit card spending). Purchase price is $120M on $15M EBITDA (8x). Should they proceed?"

**Exhibit:**
| Metric | Data Vendor |
|---|---|
| Revenue | $40M |
| EBITDA margin | 37% |
| Revenue growth (3yr avg) | 6% |
| Customer concentration | Top 3 clients = 55% revenue |
| Data refresh lag | 45 days |

**Key risks:** Customer concentration, data freshness (competitors at 3–7 days lag), regulatory risk (CFPB scrutiny of card data), integration complexity.

**Synergy angle:** Acquiring firm can embed data into deliverables, improve pricing power, reduce third-party data spend.

---

## GR01 — Consulting firm revenue growth (Growth / Medium / No)

**Prompt:**
"A mid-size strategy consulting firm ($200M revenue) wants to grow to $350M in 3 years. What's the strategy?"

**Standard Ansoff play:** Existing clients (more services, deeper relationships) vs. new clients (new industries, geographies) vs. new offerings (data/analytics practice, technology).

---

## GR02 — Marketing analytics platform growth (Growth / Hard / Yes)

**Prompt:**
"A marketing analytics SaaS ($80M ARR, growing 18% YoY) is seeing growth decelerate. The board wants to re-accelerate to 30%+. What levers exist?"

**Exhibit:**
| Segment | ARR | Growth | NRR |
|---|---|---|---|
| Enterprise | $52M | 12% | 118% |
| Mid-market | $20M | 28% | 104% |
| SMB | $8M | 45% | 87% |

**Key insight:** Enterprise is the anchor but decelerating; SMB is growing fast but churning. Net expansion strategy in enterprise (NRR 118%) is the highest-quality growth. Mid-market is the sweet spot.

**Analytics twist:** "How would you build a leading indicator model to predict which enterprise accounts are expansion candidates?"

---

## OP01 — ETL pipeline throughput failure (Operations / Hard / Yes)

**Prompt:**
"A data engineering team at a financial services firm is missing SLA targets — pipelines that used to complete in 4 hours now take 11–14 hours. This is delaying downstream reporting for 200+ internal users. What's happening and how do you fix it?"

**Available data (share if asked):**
- Data volume grew 3x in 8 months (new client onboarding)
- Pipeline architecture: sequential jobs, no partitioning
- Cloud compute: fixed cluster size (not autoscaling)
- Error logs show: frequent OOM errors on the transform step

**Key insight:** Architecture didn't scale with volume. Sequential + fixed compute = linear degradation. Fix: partitioned parallel processing, autoscaling, job-level SLA monitoring.

**This is Andrew's world** — push on specifics: "How would you redesign this in BigQuery? What partitioning strategy? How do you instrument pipeline health?"

---

## OP02 — Insurance claims processing backlog (Operations / Medium / Moderate)

**Prompt:**
"An insurance company's claims processing time went from 8 days to 19 days over 6 months. Claim volume is up 15%, staff is up 10%. What's going on?"

**Exhibit:**
| Step | Avg Time (prev) | Avg Time (now) |
|---|---|---|
| Initial intake | 1 day | 1.5 days |
| Document review | 2 days | 6 days |
| Adjuster review | 3 days | 4 days |
| Approval/denial | 2 days | 7.5 days |

**Key insight:** Document review and approval are the bottlenecks — not intake or adjuster work. Likely causes: new document types, manual review processes that didn't scale, or approval authority bottleneck (single approver?).

---

## PP01 — Pricing a litigation analytics SaaS (Pricing / Hard / Yes)

**Prompt:**
"You've built a SaaS platform that automates document ingestion, entity extraction, and timeline generation for litigation support teams at law firms. Current pricing: $5,000/month flat. You're leaving money on the table. How do you re-price?"

**Context:** Platform processes 10K–500K documents per matter. Time savings: ~40 hrs/matter for senior associates ($400/hr). Competing with manual review + standard eDiscovery tools.

**Pricing frameworks to explore:** Usage-based (per document/per GB), value-based (% of time saved), tiered by matter size, outcome-aligned (contingency-adjacent).

**Key insight:** Value-based pricing is the right anchor. At 40hrs × $400 = $16K value per matter, flat $5K/month is drastically underpriced for large matters. Tiered by document volume + matter size unlocks 3–5x revenue.

**This is Andrew's world** — push hard.

