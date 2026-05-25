# Oportun STAR Stories

*Oportun: Omnichannel AI agent platform serving 1M+ customers across SMS, web chat, email, and voice. Agentic RAG architecture. Phased rollout from 0 to production. Evaluation framework built from scratch.*

---

## Story 1 — Building the AI Agent Platform (Flagship)

**Situation**: Oportun's customer support operation was scaling linearly with headcount — every new loan origination cohort added proportional servicing cost. We were serving 1M+ active borrowers, the majority of whom were underserved, Spanish-speaking, low-income customers with high servicing contact rates (payment questions, hardship requests, account status). Human agent capacity was becoming the binding constraint on our growth economics.

**Task**: I was brought in to own the AI agent platform end-to-end — strategy, architecture decisions, vendor selection, rollout framework, and the evaluation criteria that would determine when we were ready to expand scope from pilot to production.

**Action**:
- Defined the architecture: agentic RAG rather than scripted chatbot. The customer's question needed to be understood in context (what's their account status, payment history, program eligibility), retrieved from a knowledge base (policy docs, product rules), and resolved through action (modifying payment arrangements, enrolling in hardship programs) — this required a reasoning layer, not a decision tree
- Built the intent taxonomy from scratch: analyzed 3 years of agent call recordings to identify the 47 distinct intent categories across 4 channels, then prioritized the 12 high-volume, lower-risk intents for the initial launch scope
- Designed the evaluation framework: model metrics (intent accuracy, slot fill precision, confidence calibration), product metrics (containment rate, resolution rate, CSAT by AI vs. human), business metrics (cost per interaction, 30-day repeat contact rate, regulatory compliance audit pass rate)
- Implemented a phased rollout: 5% → 15% → 40% → 100% traffic per intent, with hard go/no-go criteria at each gate. Criteria: intent accuracy >92%, containment rate within 5% of human baseline, zero FDCPA-flagged interactions in shadow mode
- Navigated the compliance architecture: every AI decision that touched consequential outcomes (payment deferral, credit actions) required human oversight. Built the audit trail from day one — a requirement that paid off 8 months later during a CFPB inquiry

**Result**: Platform launched and scaled to 1M+ customer interactions across SMS, web chat, email, and voice. Containment rate reached 68% at full rollout. Cost per AI-handled interaction was 85% lower than human-handled equivalent. The 30-day repeat contact rate issue (identified 6 months in) was diagnosed and resolved by adding proactive hardship program enrollment to the AI action set — drove a 40% reduction in repeat contacts for the affected intents.

**Transferable lesson**: Production AI is not a model problem — it's a system problem. The model is one component. The evaluation framework, the rollout discipline, the human escalation design, and the audit infrastructure are what determine whether an AI product earns operational and regulatory trust.

---

## Story 2 — The Containment vs. Resolution Distinction

**Situation**: Six months post-launch, our containment rate was 68% — above the 65% target. Leadership was celebrating the numbers. I had a nagging concern: repeat contact rates were elevated for three specific intents that, on paper, were being "contained."

**Task**: Determine whether the AI was actually resolving customer issues or just deferring them, and fix the root cause without rolling back the containment gains we'd worked hard to build.

**Action**:
- Pulled a stratified sample: 200 AI-handled conversations from each of the three flagged intents (payment plan modifications, hardship eligibility questions, credit limit inquiries)
- Manual review with the operations team: identified a consistent pattern — the AI was completing the transactional action correctly (the payment plan was modified, the hardship form was submitted) but wasn't addressing the underlying need. These customers were often in financial stress; their "payment modification" call was often a precursor to a hardship request. Human agents had learned to detect this and proactively offer hardship enrollment. The AI hadn't
- Re-defined resolution criteria for these intents: added a 30-day no-repeat-contact condition to what counted as a "resolved" interaction
- Added a "proactive eligibility check" to the AI's action set: after completing a payment modification, the AI now checks hardship program eligibility and offers enrollment to qualifying customers
- Updated the training data with labeled examples of agent conversations that demonstrated the proactive offer pattern

**Result**: 30-day repeat contact rate for the three affected intents dropped 40% over two quarters. The broader impact: I changed the team's product philosophy. We updated the OKR definition — containment rate is a leading indicator, resolution rate (30-day no-repeat-contact) is the goal. All subsequent intent launches included the downstream metric from the start.

**Transferable lesson**: The metric you optimize for becomes the behavior you get. If you define success as "did the AI handle it without escalating to a human?", you get high containment. If you define success as "did the customer's problem actually get solved?", you get a harder but more honest measurement framework.

---

## Story 3 — Regulatory Architecture as Product Design

**Situation**: Nine months into the platform build, the CFPB announced enhanced scrutiny on AI-assisted customer interactions in the lending sector. The compliance team came to me with a list of audit requirements: full transcript logging, decision trail for every AI action, explainability for credit-adjacent decisions, and a remediation process for customers who were negatively impacted by an AI error.

**Task**: Incorporate these requirements into the live production system without halting the rollout and without creating a compliance infrastructure that was too brittle to maintain.

**Action**:
- Mapped each CFPB requirement to a specific system component: transcript logging → conversation history database; decision trail → action log with model confidence scores; explainability → policy citation injection in every AI response (the AI cites the specific policy it's referencing); remediation → human review queue for interactions flagged by the confidence anomaly detector
- Identified what we'd built right: the audit trail was already in place (I'd insisted on it in the initial architecture), and the policy citation pattern was already embedded in the prompt design. What was missing: a formal remediation workflow and a regulatory reporting format
- Built the remediation workflow as a product feature, not a compliance workaround: customers who received an AI response with a policy citation could request a human review of that specific decision. This gave us a customer-facing transparency mechanism AND generated labeled correction data for model improvement
- Created a regulatory reporting dashboard that extracted relevant metrics in the format CFPB requested

**Result**: When the CFPB inquiry came — 8 months later — we produced a complete audit trail for the 500 sampled interactions in 4 hours. No findings against Oportun's AI system. The remediation workflow also generated 400+ human-reviewed corrections that went back into the training pipeline, improving Tier 3 intent accuracy by 2.3 percentage points.

**Transferable lesson**: In regulated industries, compliance infrastructure built after the product is shipped is always more expensive and more disruptive than compliance built into the design. The constraint is a product requirement, not an obstacle to product development.

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../../README.md)*
