# Case Bank

40 case prompts across types. Select by type + difficulty. Analytics-weighted for this user.

---

## DATA ANALYTICS CASES (Priority)

### Root-Cause / KPI Decline

**DA-01** [STANDARD]  
You're the analytics lead at a mid-size e-commerce company. The head of product comes to you on Monday: "Our add-to-cart rate dropped 22% week-over-week. Marketing is blaming the new checkout UI. Engineering says nothing changed. Figure out what happened."  
*Where do you start? What hypotheses do you form before pulling a single query?*

**DA-02** [HARD]  
You support a litigation matter involving a fintech lender. Loan approval rates for a protected class dropped 14% in Q3 relative to Q2 — but aggregate approval rates were flat. The client says it's a data artifact. How do you evaluate that claim?  
*What analyses do you run? What would confirm or disprove the "artifact" theory?*

**DA-03** [WARM-UP]  
A B2B SaaS company's monthly active users dropped 8% last month. You have access to login logs, feature usage tables, and the CRM. Walk me through your diagnostic approach.

**DA-04** [HARD]  
You're supporting an IP litigation matter. The opposing party claims their competing product was developed independently. You have: git commit logs (timestamps, authors), patent filing dates, and a dataset of internal search queries. What story can the data tell, and what are the limitations?

**DA-05** [STANDARD]  
An e-commerce client's revenue is flat YoY but margins dropped 400bps. The CFO says "cost increase." The CMO says "mix shift." How do you decompose this and figure out who's right?

---

### Metrics Design

**DA-06** [STANDARD]  
A company is launching a new content recommendation algorithm. The PM asks you: "What metrics should we use to know if it's working?" Design the measurement framework — north star, guardrail metrics, and leading indicators.

**DA-07** [WARM-UP]  
You're building a dashboard for a law firm's e-discovery practice. What metrics matter, and how would you prioritize them?

**DA-08** [HARD]  
A client wants a single "health score" for their 200-person sales team. They want it rolled up to the VP level daily. What are the risks of this approach, and how would you design it to be useful rather than misleading?

---

### Experiment / A/B Testing

**DA-09** [STANDARD]  
A product team ran an A/B test on a new onboarding flow. Treatment group: 5,200 users. Control: 5,100. After 2 weeks: treatment conversion 12.3%, control 11.8%. p-value = 0.09. The PM wants to ship. What do you say?

**DA-10** [HARD]  
Your company ran a pricing experiment: 10% of users saw a 15% price increase. Revenue went up 12%. The CEO wants to roll it out. Walk me through every question you'd ask before giving a recommendation.

**DA-11** [WARM-UP]  
You just inherited a test that's been running for 6 months on a low-traffic page. How do you evaluate whether the results are trustworthy?

---

### Data Quality / Forensics

**DA-12** [STANDARD]  
You run a query on a transaction dataset and notice that 3% of rows have null values in a key field — but only for records after a specific date. What happened, and how do you handle it in your analysis?

**DA-13** [HARD]  
A client's financial model shows revenue recognition patterns inconsistent with their stated billing cycles. Specific months show spikes with no corresponding increase in contracts signed. What are your hypotheses, and how do you investigate this forensically?

**DA-14** [STANDARD]  
You're onboarding a new data source from a third-party vendor. You have a data dictionary, 3 months of raw data, and no documentation on ETL logic. How do you validate this data before using it in production?

---

### Cohort / Funnel Analysis

**DA-15** [WARM-UP]  
Conversion through a 4-step checkout funnel is: Step 1→2: 82%, Step 2→3: 71%, Step 3→4: 63%, Step 4 (purchase): 41%. Where do you focus first, and what additional data do you want?

**DA-16** [STANDARD]  
User retention for a mobile app: Month 1: 45%, Month 3: 28%, Month 6: 12%. The industry benchmark for Month 6 is 22%. Walk me through how you'd diagnose and prioritize fixes.

**DA-17** [HARD]  
Cohort analysis shows that users acquired via paid social have 30% higher 30-day retention than organic users — but 40% lower 90-day retention. How do you interpret this? What business decisions does it affect?

---

### SQL / Logic Reasoning

**DA-18** [STANDARD]  
You have a `transactions` table (user_id, amount, created_at, status) and a `users` table (user_id, signup_date, plan_type). A stakeholder asks: "Which users made their first purchase within 7 days of signup?" Describe the logic and any edge cases you'd flag.

**DA-19** [HARD]  
You're asked to calculate 30-day rolling retention. The data is in BigQuery. The `events` table has user_id, event_type, and event_timestamp. Walk me through your approach — and flag any gotchas specific to BigQuery.

**DA-20** [WARM-UP]  
Given a table with `order_id`, `customer_id`, `order_date`, `revenue` — how do you find customers who've made at least 3 purchases in the last 90 days?

---

## PROFITABILITY CASES

**P-01** [WARM-UP]  
A regional grocery chain's operating profit dropped 18% YoY. Revenue was flat. Where do you start?

**P-02** [STANDARD]  
A SaaS company had record revenue last quarter but missed EBITDA targets by $4M. The CFO needs to understand why before the board meeting. Walk me through your framework.

**P-03** [HARD]  
A consulting firm's profit per engagement is down 22% despite flat headcount and a 10% revenue increase. Diagnose. (Curveball mid-case: utilization data shows only a 2% drop.)

**P-04** [STANDARD]  
A manufacturer's gross margin dropped from 38% to 31% in 18 months. They've had two product lines, stable pricing, and no major supply shocks. What's your hypothesis tree?

---

## MARKET ENTRY / GROWTH

**ME-01** [STANDARD]  
An analytics consulting firm based in Chicago is considering expanding into a new vertical: healthcare litigation. Should they? Walk me through the decision.

**ME-02** [HARD]  
A private equity firm is evaluating a $50M investment in a data pipeline company targeting the adtech space. What framework do you use to assess the opportunity? What would make you pass?

**ME-03** [WARM-UP]  
A legal tech startup wants to expand from e-discovery into contract analytics. What do you want to know before recommending they go or no-go?

---

## M&A / INVESTMENT

**MA-01** [STANDARD]  
A litigation support firm is considering acquiring a boutique forensic accounting firm. The target has $8M revenue, 80% gross margins, and 6 key-person dependencies. Evaluate.

**MA-02** [HARD]  
You're advising a PE firm on acquiring a healthcare analytics startup. Revenue is $12M growing 40% YoY but churn is 25%. Build a framework for evaluating whether this is a good deal.

---

## MARKET SIZING / ESTIMATION

**MS-01** [WARM-UP]  
How many data analysts work in the Chicago metro area?

**MS-02** [STANDARD]  
Estimate the annual spend on litigation support analytics services in the US.

**MS-03** [HARD]  
A client wants to know the addressable market for a SaaS product that automates expert witness report generation using LLMs. Size it.

---

## OPERATIONS / PROCESS

**OP-01** [STANDARD]  
A document review team of 12 analysts is missing turnaround time SLAs on 30% of matters. You have access to matter-level data (doc volume, reviewer assignment, completion timestamps). How do you diagnose this?

**OP-02** [HARD]  
A GCP-based batch processing pipeline is completing only 60% of jobs before hitting a daily cost cap. Engineering says "scale up compute." Your job is to challenge that assumption analytically before they spend $200K. What do you look at?

---

## PRICING

**PR-01** [STANDARD]  
A litigation support firm charges clients by the hour. A new competitor is offering fixed-fee engagements. Should your client switch their pricing model? What data do you need?

**PR-02** [HARD]  
A data products company is launching a new API product. Customers range from solo analysts to Fortune 500 teams. Design a pricing framework that maximizes revenue without cannibalizing enterprise deals.

