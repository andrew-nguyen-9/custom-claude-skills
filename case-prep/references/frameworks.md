# Framework Deep-Dives

## When to load this file
Load when doing a framework drill, or when a candidate's structure needs coaching in debrief.

---

## Profitability Framework

```
Profit = Revenue - Cost

Revenue
├── Price
│   ├── List price changes?
│   ├── Discount/promo increases?
│   └── Mix shift to lower-priced products?
└── Volume
    ├── Units sold (market size × share)
    ├── Market growth / contraction?
    └── Share gain / loss?

Cost
├── Fixed
│   ├── Overhead, rent, headcount
│   └── D&A, interest
└── Variable
    ├── COGS (materials, labor, logistics)
    └── Sales & marketing, support
```

**Common traps:**
- Confusing gross margin decline with operating income decline
- Not separating mix effects from rate effects
- Ignoring one-time items (restructuring, write-downs)

**Analytics overlay:** Always ask about revenue recognition methodology (accrual vs. cash) and whether cost allocation changed.

---

## Market Sizing Framework

**Top-Down:**
Total addressable population → Segment of interest → Penetration rate × Avg spend = Market size

**Bottom-Up:**
Units of supply (stores, factories, providers) × Capacity × Utilization × Price = Market size

**Sanity check:** Cross-validate with the other method. If they diverge >2x, find the assumption gap.

**Common traps:**
- Conflating TAM with SAM or SOM
- Ignoring seasonality or one-time demand
- Failing to define the market boundaries clearly

**Data anchor points to memorize:**
- US population: ~335M
- US households: ~130M
- US internet users: ~290M (~87%)
- US smartphone users: ~275M
- US adults 18-65: ~195M

---

## Market Entry Framework

**Layer 1: Market Attractiveness**
- Size: Is the TAM large enough to matter?
- Growth: Is the market growing or declining?
- Profitability: What are industry margins? Why?
- Competition: Concentrated? Commoditized? Disruption risk?
- Barriers to entry: Regulation, capital, IP, switching costs

**Layer 2: Company Fit**
- Capabilities: Do we have the skills/tech/distribution?
- Financial: Can we fund the entry? What's payback period?
- Strategic: Does this fit our core business or distract from it?
- Risk: What could go wrong? Can we exit if needed?

**Decision framework:**
High attractiveness + high fit → Enter aggressively
High attractiveness + low fit → Consider acquisition or partnership
Low attractiveness + high fit → Enter selectively or wait
Low attractiveness + low fit → Don't enter

---

## M&A Framework

**Step 1: Strategic rationale**
- Market consolidation / scale?
- Capability acquisition (technology, talent, IP)?
- Vertical integration?
- Geographic expansion?

**Step 2: Financial return**
- Valuation multiple: Is it fair?
- Synergies: Revenue synergies (cross-sell, pricing) + Cost synergies (overlap elimination)
- IRR/NPV at different scenarios
- Financing structure (cash, stock, debt implications)

**Step 3: Integration feasibility**
- Culture fit
- Technology/systems compatibility
- Key talent retention risk
- Regulatory approval likelihood

**Common traps:**
- Overestimating synergies (especially revenue synergies)
- Underestimating integration costs and distraction
- Ignoring customer concentration risk in target

---

## Operations / Process Framework

**Input → Process → Output**

Diagnose by:
1. Is input volume the problem? (demand spike, new SKUs, system failures upstream)
2. Is process the problem? (bottleneck step, sequential vs. parallel, manual steps)
3. Is output the problem? (quality issues, rework loops, approval delays)

**Tools:**
- Process mapping (find the bottleneck)
- Capacity analysis (demand vs. throughput)
- Queue theory (utilization → wait time is non-linear above ~85% capacity)

**In a data engineering context:**
- Input: data sources, volumes, formats
- Process: ingestion → transform → load (identify slowest step)
- Output: downstream SLAs, query performance, freshness

---

## Growth Strategy Framework (Ansoff)

|  | Existing Products | New Products |
|---|---|---|
| **Existing Markets** | Market Penetration (best ROI) | Product Development |
| **New Markets** | Market Development | Diversification (highest risk) |

**Prioritization:**
1. First, saturate existing market with existing products (lowest risk, best unit economics)
2. Then expand existing products to adjacent markets
3. Only pursue new product development with strong capability rationale
4. Avoid diversification unless core business is truly saturated

---

## Pricing Framework

**Three anchors:**
- Cost-plus: Floor (don't price below fully-loaded cost)
- Value-based: Ceiling (price up to value delivered to customer)
- Competitive: Market anchor (what are substitutes charging?)

**Pricing model options:**
- Flat subscription: Predictable, but leaves money on table at high usage
- Usage-based: Aligns with value, but unpredictable for customer
- Tiered: Segmentation play — captures willingness to pay across segments
- Outcome-based: Highest alignment, hardest to implement
- Freemium: Land-and-expand, requires strong upsell motion

**Analytics SaaS specifically:**
- Key metric to price on: queries, seats, data volume, events processed, or outcomes (e.g., cost savings, revenue lifted)
- NRR implications: Usage-based enables expansion revenue without sales effort

