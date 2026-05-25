# Conversational AI Metrics Reference

*The metrics that actually matter for measuring conversational AI product quality — drawn from running IVR and AI agent systems at Discover and Oportun.*

---

## The Metric Hierarchy

### Tier 1: Leading Indicators (Real-time monitoring)
These move fast and signal a problem before it compounds.

| Metric | Definition | Alert Threshold |
|--------|-----------|-----------------|
| Model confidence distribution | Mean confidence score across all interactions in the last hour | Alert if mean drops >10% below rolling 7-day average |
| Intent classification rate | % of interactions where the AI successfully classified an intent (vs. "I don't understand") | Alert if drops >5% from baseline |
| Error rate | % of API calls returning errors or timeouts | Alert if >1% over 5-minute window |
| Latency P95 | 95th percentile response time | Alert if exceeds 3x SLA target |

### Tier 2: Product Quality (Daily review)
These reflect whether the AI is actually helping users.

| Metric | Definition | Healthy Range |
|--------|-----------|---------------|
| Containment rate | % of sessions resolved without human escalation | Context-dependent; track trend vs. target |
| Escalation trigger distribution | Why interactions escalate: low confidence, user request, scope limit, error | Track by reason — "user request" escalation rising = different problem than "low confidence" rising |
| Conversation length | Median turns to resolution | Should decrease as the model improves; sudden increase signals a regression |
| Intent accuracy (sampled) | Sample 2% of interactions weekly; human-label the correct intent; compare to model output | Target >90% on sampled set |
| Slot fill precision | Of extracted entities, % that are correct | Target >92% |

### Tier 3: Business Outcomes (Weekly/Monthly)
These determine whether the product justified the investment.

| Metric | Definition | What Movement Means |
|--------|-----------|---------------------|
| Resolution rate | % of AI-handled sessions with no repeat contact within 30 days | Primary indicator of actual problem-solving quality; different from containment rate |
| Cost per interaction | Fully loaded cost (model inference + infrastructure + amortized human review) per AI-handled interaction | Should trend down as volume scales; sudden spikes = infrastructure issue |
| CSAT delta | CSAT score for AI-handled interactions vs. human-handled interactions (matched by intent) | Target: AI within 0.3 points of human; gap widening = quality regression |
| 30-day repeat contact rate | % of AI-resolved interactions where same user contacts again within 30 days | The single best indicator of real resolution vs. apparent resolution |
| Agent time savings | Hours saved by human agents due to AI containment | Multiply (contained interactions) × (average agent handle time) |

---

## Common Measurement Mistakes

### Optimizing Containment Rate Without Resolution Rate
**The mistake**: The AI keeps interactions from reaching human agents (high containment) but doesn't actually solve the customer's problem. Customers call back the next day — possibly angrier.

**Why it happens**: Containment is easy to measure in real-time; resolution requires a 30-day lag. Teams optimize for the metric they can see.

**The fix**: Add resolution rate (30-day no-repeat-contact) to the success framework from day one. Don't declare a feature successful until resolution data is available.

### Using Test Set Accuracy as a Production Proxy
**The mistake**: The model achieves 94% intent accuracy on the test set. The team launches. In production, intent accuracy is 81% because real user language differs from the test set.

**Why it happens**: Test sets are curated from historical data; production traffic includes novel phrasing, code-switching, background noise (for voice), and edge cases that weren't in the training distribution.

**The fix**: Shadow mode evaluation on real traffic before launch. Measure accuracy on a 2% sample of actual production interactions, not just the test set.

### Aggregating Metrics Across Heterogeneous Intent Categories
**The mistake**: Overall intent accuracy is 91%. The team is satisfied. One intent category — payment disputes — has 64% accuracy, but its volume is small enough that it doesn't move the aggregate.

**Why it happens**: Aggregate metrics hide tail failures. The intents with lowest accuracy are often the highest-stakes ones (because they're harder).

**The fix**: Report all quality metrics by intent category, not just in aggregate. Set per-intent thresholds that must be met, not just the overall average.

### Counting "Didn't Escalate" as Resolution
**The mistake**: The AI answers the customer's question. The customer doesn't escalate. The interaction is counted as "resolved." But the AI gave incorrect information — the customer just didn't know enough to challenge it.

**Why it happens**: Escalation is easy to measure; factual accuracy requires human review.

**The fix**: For high-stakes intent categories (credit-adjacent, regulatory-adjacent), add a human audit sample to the evaluation framework. 2% of interactions reviewed by a human reviewer prevents the systematic drift toward confident-but-wrong outputs.

---

## A/B Testing Framework for AI Features

### When to A/B Test
A/B test when: changing model version, changing confidence thresholds, adding new intent categories, changing escalation logic, or launching the AI feature for the first time (vs. incumbent system).

### Treatment and Control Design
- **Control**: existing system (IVR, human agents, or previous AI version)
- **Treatment**: new AI feature
- **Assignment**: session-level (not user-level) for most conversational AI experiments — users may contact multiple times; session-level assignment avoids contamination
- **Traffic split**: 5-10% to treatment initially; scale up as confidence grows

### Required Sample Sizes
Intent accuracy test (detecting a 3% absolute improvement in intent accuracy at 80% power, 95% confidence):
- ~2,500 interactions per intent category per arm
- Rule of thumb: run for minimum 2 weeks regardless of sample size to account for weekly seasonality

### Pre-registered Metrics
Define before the experiment starts (not after seeing results):
1. Primary: containment rate, intent accuracy
2. Secondary: CSAT, conversation length, escalation rate
3. Guardrail: compliance flags, error rate, latency

**Hard rule**: if a guardrail metric worsens during the experiment, pause — regardless of what the primary metric shows.

---

## Evaluation Metrics Quick Reference

| Metric | Formula | When to Use |
|--------|---------|-------------|
| Accuracy | Correct / Total | Single-class classification |
| Precision | TP / (TP + FP) | When false positives are costly |
| Recall | TP / (TP + FN) | When false negatives are costly |
| F1 | 2 × (P × R) / (P + R) | When you need balance between precision and recall |
| Containment Rate | AI-resolved / Total sessions | Conversational AI efficiency |
| Resolution Rate | No-repeat-contact (30d) / AI-resolved | Conversational AI quality |
| CSAT Delta | CSAT(AI) - CSAT(human) | User experience comparison |
| Cost per Interaction | (Inference + infra cost) / AI sessions | Unit economics |
| MRR | (1/rank of first relevant result) averaged across queries | Retrieval quality |
| Recall@K | Relevant docs in top K / Total relevant docs | RAG retrieval completeness |

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../README.md)*
