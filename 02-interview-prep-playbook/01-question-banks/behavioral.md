# Behavioral — AI PM Question Bank

*Framework: STAR (Situation → Task → Action → Result). Every story should end with a quantified outcome and a transferable lesson — what would I do the same, and what would I do differently.*

---

## Q: Tell me about yourself / Walk me through your background.

**My answer (60-second version)**:

I'm an AI PM with a specific background: I've built and shipped production AI systems in regulated fintech — not just prototyped them, actually shipped them to millions of customers.

At Discover, I launched a Natural Language IVR using Google CCAI and Dialogflow CX — it handled 5M+ calls per year and delivered $7M in annual savings by lifting containment from 35% to 65%. That was my first large-scale conversational AI deployment.

At Oportun, I scaled that up significantly — I owned an omnichannel AI agent platform serving 1M+ customers across SMS, web chat, email, and voice. The architecture was agentic RAG: the AI could retrieve knowledge, take actions against core systems, and escalate to human agents when needed. I built the evaluation framework, designed the phased rollout, and defined what "resolution" meant for a regulated lending environment.

The through-line: I've solved the hard problems — not just "can the AI answer the question?" but "what happens when it's wrong?", "how do we evaluate it at production scale?", and "how do we earn regulatory sign-off on an autonomous system?"

What brings me here: I want to work on AI products where the technical depth and the business complexity are both high — and where the product decisions I make will affect how real people interact with AI.

---

## Q: Tell me about a time you shipped an AI feature that didn't go as planned.

**Situation**: Six months into the Oportun AI agent rollout, our containment rate was 68% — above target — but our 30-day repeat contact rate was elevated for three specific intent categories: payment plan modifications, hardship program eligibility questions, and credit limit inquiries.

**Task**: I needed to understand whether the AI was actually resolving these interactions or just deferring them, and fix the product without rolling back the overall containment gains.

**Action**: I pulled a sample of 200 conversations from each intent category and did a manual review with the operations team. The pattern: the AI was giving technically correct answers — the payment plan modification was being processed — but wasn't addressing the underlying reason the customer called. A customer asking to modify a payment plan was often signaling financial stress; the AI processed the transaction but didn't surface hardship program eligibility information that the customer would have qualified for. Agents had learned to do this through experience; the AI didn't have that contextual judgment.

I re-designed the resolution criteria for those three intents to include a downstream check — did the customer call back within 30 days? — and added a "proactive offer" action to the agent's action set: when a payment modification was processed, the AI would check eligibility for hardship programs and offer enrollment. I also updated the training data to include agent conversations that demonstrated this pattern.

**Result**: 30-day repeat contact rate for those three intents dropped 40% over the next two quarters. The broader lesson I built into the product: "containment" is a proxy metric, not the goal. The goal is customer resolution. I updated the team's definition of success metrics and added downstream metrics to all future intent launches.

---

## Q: Tell me about a time you had to influence without authority on an AI decision.

**Situation**: At Discover, 8 months into the NL IVR project, the call center operations team wanted to reduce the AI's confidence threshold from 0.75 to 0.60 to increase containment rate and hit the cost savings target. My analysis showed that at 0.60, we'd also dramatically increase the rate of incorrect intent classifications — routing customers to the wrong IVR path and forcing them to restart the call. The ops team didn't have full visibility into the error distribution; they were optimizing for a single metric.

**Task**: I needed to prevent a decision that would hurt customer experience — without having direct authority over the ops team's decision or the call center's performance targets.

**Action**: I built the argument in terms the ops team cared about — cost. I pulled the call data: every incorrect routing at 0.60 threshold would generate a repeat call at ~$4.50 per call (fully loaded contact center cost). At the projected error rate increase, the repeat calls would offset 60% of the containment gain. I modeled three scenarios: 0.60 threshold (high containment, high error rate, net savings = $2.1M), 0.75 threshold (current, net savings = $4.2M), and a hybrid approach I proposed (0.75 for high-stakes routing decisions, 0.60 for low-stakes informational intents) that achieved $5.8M in net savings by applying the lower threshold only where error cost was low.

I presented this to the VP of Operations and the VP of Technology together — made it a data conversation, not a judgment call. The hybrid approach was approved.

**Result**: We implemented the tiered threshold approach. Net savings reached $5.9M in year 1, exceeding the original $7M 3-year target in year 2. The ops team adopted the tiered threshold framework as standard practice for all future AI routing decisions.

---

## Q: Describe a time you had to make a product decision with incomplete data.

**Situation**: At Oportun, we were evaluating whether to expand the AI agent to handle payment hardship program enrollments — a high-stakes intent where the customer outcome (approved or denied for a forbearance program) had material financial consequences. We had strong AI performance data for the simpler intents we'd already shipped, but no data on hardship program handling specifically.

**Task**: Decide whether to expand scope now (capitalize on momentum, hit roadmap dates) or wait for more data (risk delay, but protect against shipping a faulty system for high-stakes decisions).

**Action**: I couldn't get direct performance data without a live experiment, and I couldn't run a live experiment without a go/no-go decision. I broke the decision into components: (1) What was the cost of a wrong AI decision here? — a denied customer who should have been approved could default; an approved customer who shouldn't have been created credit risk. Both had measurable downstream costs. (2) What data could I get as a proxy? — I pulled agent call recordings for hardship conversations, had the AI classify the intent and recommended outcome on historical data, and measured against actual agent decisions. (3) What guardrails could reduce downside? — I proposed a "recommendation only" mode: the AI presents the eligibility determination to the human agent who makes the final decision, rather than acting autonomously.

**Result**: I recommended the recommendation-only launch with a 90-day evaluation period. After 90 days, AI recommendations matched agent decisions at 94% accuracy. We then moved to autonomous handling for clear-approval cases (AI score > 0.90) and kept human review for borderline cases. The staged approach gave us the data we needed without full risk exposure, and the final system performed better than a direct autonomous launch would have because the 90-day learning period improved the model.

---

## Q: Tell me about a time you disagreed with your engineering team on a technical AI decision.

**Situation**: At Oportun, the engineering team proposed using a single GPT-4-based model for all intents on the AI agent platform — simpler architecture, faster to ship, one model to evaluate and maintain.

**Task**: I believed a multi-model architecture — routing intents to different models based on complexity and risk — was the right product decision, but the engineering team had legitimate reasons for wanting simplicity.

**Action**: I didn't fight the architecture decision directly. I proposed a structured experiment: define three intent tiers by complexity and risk (Tier 1: simple informational, Tier 2: transactional, Tier 3: high-stakes/complex), run the single GPT-4 model on all three, measure performance by tier, and evaluate whether tiered models would improve the critical metrics for Tier 3 (where error cost was highest).

The data showed GPT-4 performed well on Tier 1 and 2, but had an 11% error rate on Tier 3 intents — above our 5% threshold. I used that data to make the case for a targeted intervention: not a complete rebuild, but a specialized model for Tier 3 with the existing GPT-4 model handling everything else. Engineering agreed because I was proposing a minimal change backed by evidence, not a full re-architecture based on instinct.

**Result**: The Tier 3 specialized model reduced error rate from 11% to 4.2%. Total system performance improved without the complexity of a full multi-model rebuild. The lesson I took: disagreements with engineering teams go better when you propose experiments rather than conclusions.

---

## Q: How do you work with data scientists and ML engineers?

**My approach**: I treat them as full product partners, not implementation resources. That means: (1) I come to technical conversations with a clear problem definition and success criteria, not a feature spec. "Improve accuracy" is not a product requirement. "Achieve <5% false negative rate on payment deferral intent while maintaining >92% precision" is. (2) I invest time understanding the model — I don't need to write the code, but I need to understand why a model makes specific errors, what the evaluation metrics mean, and what data changes would improve performance. (3) I protect them from scope creep and stakeholder noise — my job is to translate product requirements into model requirements, and to translate model limitations into stakeholder expectations.

**What doesn't work**: PMs who hand off a vague requirement and expect ML engineers to also define the success criteria. Success criteria is a product decision, not an ML decision — the PM owns it.

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../../README.md)*
