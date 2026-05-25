# Stripe — Company-Specific Interview Prep

**Role target**: AI PM / Product Manager, AI Products
**Why Stripe makes sense for my background**: Stripe operates at the intersection of fintech infrastructure, developer tooling, and enterprise scale — all three areas where I have depth. The AI products angle is the new surface: Stripe is deploying AI for fraud detection, revenue optimization, and financial operations automation. My fintech regulatory background (FCRA, ECOA, FDCPA) and production AI experience are directly relevant.

---

## Company Context

**What Stripe does**: Payment infrastructure for businesses of all sizes — from indie developers to Fortune 500s. Core products: Payments, Billing, Connect (marketplace), Atlas, Radar (fraud), Treasury, and Sigma (analytics).

**Stripe's AI surface**:
- **Radar**: ML-based fraud detection — adaptive rules, risk scores on every transaction, machine-learning models trained on Stripe's global transaction network
- **Financial connections + AI**: intelligent categorization and reconciliation of financial data
- **Stripe AI (2025)**: AI-native financial operations features — smart revenue recovery, automated financial reporting, anomaly detection
- **Internal AI tooling**: Stripe is a heavy internal AI user — engineering copilots, support automation, doc generation

**Revenue**: Private; estimated $3-4B+ ARR. $95B valuation (2023 secondary market). Processing hundreds of billions in payment volume annually.

**Engineering culture**: Known for extremely high bar, API-first design philosophy ("designed for developers"), strong opinions on simplicity and correctness. Stripe docs are famously excellent — a product signal.

---

## Why I'm a Fit

- **Fintech production experience**: I've shipped AI in regulated fintech (Oportun, Discover), not just adjacent tech. I understand payment flows, compliance requirements, and the cost of AI errors in financial contexts
- **Scale**: 1M+ customers on the Oportun AI agent, 5M+ annual calls on the Discover NL IVR — I've designed for scale, not just for demos
- **Developer empathy**: Stripe's primary user is a developer. I've worked closely with engineering teams to build developer-facing interfaces and internal tools; I understand the developer expectation of correctness and reliability over creativity
- **Fraud and risk context**: Stripe Radar is an ML system at its core — risk scoring, adaptive rules, false positive management. These are exactly the evaluation and trade-off frameworks I built at Oportun and Discover

---

## Questions to Expect

### "What would you build at Stripe?"

**My angle**: Stripe has billions of data points on what financial fraud looks like, what revenue recovery patterns work, and what merchant financial health signals exist. The untapped opportunity is a **Stripe Revenue Intelligence** product — AI that analyzes a merchant's revenue patterns, identifies recovery opportunities (failed payments that could be retried with different methods, subscription churn signals before they materialize, pricing optimization opportunities based on comparable merchant behavior), and surfaces them proactively.

Why this: Stripe already has the data and the trust relationship. Merchants who use Stripe already share their full financial transaction history. An AI that helps them optimize revenue — not just process payments — would increase Stripe's value per merchant and deepen the relationship from infrastructure to strategic advisor.

Success metric: Revenue recovered per merchant per month (for payment retry and churn intervention features), measured against a control group.

### "How does Stripe's flywheel work and where does AI fit?"

**My answer**: Stripe's network advantage is its data — it processes transactions across millions of merchants and billions of end-customers. This gives Radar ML models that are more accurate than any single-merchant fraud system could be (more data on fraud patterns across the network). AI amplifies this flywheel: better fraud models → lower false positive rates → merchants keep more revenue → more merchants join → more transaction data → better models.

The same flywheel applies to revenue recovery: more merchants using Stripe's retry logic → more data on what retry strategies work by payment type, geography, and customer segment → smarter retry rules for all merchants.

### "How would you prioritize Stripe's AI roadmap?"

**My framework**: Start from Stripe's core promise — reliable, developer-friendly payment infrastructure. AI features should protect or extend that promise. Priority order:

1. Features that reduce false positives in Radar (fraud detection errors hurt merchants directly — the primary risk to Stripe's trust relationship)
2. Features that reduce failed payment revenue loss (a direct monetary impact that's easy to attribute to Stripe)
3. Features that give merchants predictive intelligence (revenue analytics, churn signals) — these extend the relationship depth but aren't core to the payment infrastructure promise

### "How do you think about AI safety in payments?"

**My answer**: In financial infrastructure, safety means two things: accuracy (the model is right about what it predicts) and adversarial robustness (the model can't be manipulated by sophisticated fraud actors who understand how Stripe's risk scoring works). I'd design Stripe's AI safety framework with three pillars: offline accuracy benchmarks (precision/recall by fraud type, calibration across merchant categories), adversarial red-teaming (what attack patterns does the model fail to detect?), and false positive monitoring (what legitimate transactions are being blocked?). The last one matters most for merchant trust — a merchant who has legitimate orders declined by Radar loses revenue and may churn.

---

## Research to Do Before the Interview

- Deep-dive Stripe Radar documentation — understand how rules work, how risk scores are constructed, what merchant controls exist
- Read Stripe's engineering blog — they publish extensively on ML infrastructure, payment systems, and developer tooling philosophy
- Understand Stripe's Connect product (marketplace payments) — it's the most complex surface where AI could add value (fraud at marketplace scale, seller onboarding risk assessment)
- Review Stripe's 2024-2025 product announcements for AI features that have shipped

---

## Questions to Ask the Interviewer

- "How does Stripe balance model accuracy vs. merchant control in Radar — when a merchant disagrees with a risk score, what happens?"
- "What's the current PM-engineering ratio on the AI products team and how does that shape how product decisions get made?"
- "Where is the biggest AI opportunity you're NOT currently pursuing — and why not?"

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../../../README.md)*
