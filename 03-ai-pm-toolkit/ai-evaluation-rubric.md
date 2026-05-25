# AI Evaluation Rubric

*The evaluation framework is the product artifact that determines whether you can trust your AI system, improve it, and defend it to regulators. Build it before you build the model.*

---

## Three-Layer Evaluation Structure

Every AI product I've built uses three evaluation layers — model, product, and business. Measuring only one layer gives you a misleading picture of whether the AI is actually working.

---

### Layer 1: Model Metrics
*Is the AI performing the cognitive task correctly?*

**For classification (intent, sentiment, category)**:
| Metric | Definition | Target Range |
|--------|-----------|--------------|
| Accuracy | % of predictions that are correct | >88% for NLU classification |
| Precision | Of predicted class X, % actually class X | >90% for high-stakes classes |
| Recall | Of actual class X, % predicted as class X | >85% for regulated-action classes |
| F1 Score | Harmonic mean of precision/recall | >87% for balanced evaluation |
| Confidence Calibration | How well stated confidence matches actual accuracy | Within 10% ECE |

**For generation (responses, summaries, drafts)**:
| Metric | Definition | How to Measure |
|--------|-----------|----------------|
| Faithfulness | Does the response accurately reflect the source material? | Human eval or faithfulness model (e.g., FACTSCORE) |
| Relevance | Does the response address the actual question? | Human eval: 1-5 scale |
| Completeness | Does the response include all necessary information? | Human eval against a reference checklist |
| Hallucination rate | % of responses containing ungrounded factual claims | Automated faithfulness check + human sample review |

**For retrieval (RAG, search)**:
| Metric | Definition | Target Range |
|--------|-----------|--------------|
| Recall@K | % of relevant documents in top-K results | Recall@3 >80% |
| MRR (Mean Reciprocal Rank) | How highly ranked is the first relevant result? | >0.75 |
| Context Precision | Of retrieved chunks, % actually relevant to the query | >70% |

---

### Layer 2: Product Metrics
*Is the AI driving the intended user behavior?*

**For conversational AI / support agents**:
| Metric | Definition | Calculation |
|--------|-----------|-------------|
| Containment rate | % of sessions resolved without human escalation | (Sessions resolved by AI) / (Total sessions) |
| Resolution rate | % of sessions where user issue was actually resolved | (Sessions with no 30-day repeat contact) / (Total sessions) |
| Escalation trigger distribution | What causes handoffs: confidence, user request, intent type | Log escalation reason codes |
| Conversation length | Average turns to resolution | Total turns / resolved sessions |
| Time to resolution | Calendar time from session start to resolution | Useful for async channels (email, SMS) |

**For productivity AI (copilots, writing assistants)**:
| Metric | Definition | Notes |
|--------|-----------|-------|
| Acceptance rate | % of AI suggestions accepted by user | High acceptance ≠ good suggestions; could indicate user fatigue |
| Edit rate | % of accepted suggestions that are modified before use | Measures whether AI is generating the right thing or just close |
| Task completion time | Time to complete task with AI vs. without | Requires A/B test or historical baseline |
| Return rate | % of users who use the feature again within 7 days | Leading indicator of perceived value |

**For recommendation / personalization**:
| Metric | Definition | Notes |
|--------|-----------|-------|
| CTR (Click-through rate) | % of AI recommendations that users engage with | High CTR on bad recommendations creates satisfaction debt |
| Conversion rate | % of engaged recommendations that complete desired action | The business-level outcome |
| Coverage | % of item catalog that AI recommends | Low coverage = popularity bias |
| Diversity | Spread of recommendations across categories | Prevents filter bubbles |

---

### Layer 3: Business Metrics
*Is the AI moving the business outcome that justified the investment?*

**Cost metrics**:
- Cost per AI-handled interaction vs. cost per human-handled interaction
- Total interaction cost trend (is the mix shifting favorably?)
- Infrastructure cost per interaction (model inference cost, retrieval cost)

**Revenue metrics**:
- Revenue recovered (for retry/recovery AI)
- Conversion rate on AI-assisted vs. non-AI-assisted sessions
- Upsell/cross-sell rate where AI is in the path

**Risk metrics**:
- Regulatory compliance rate (% of AI interactions that pass audit)
- Error rate on consequential decisions (credit, payment, account actions)
- Complaint rate attributable to AI errors

**Retention metrics**:
- NPS for AI-handled interactions vs. human-handled
- 30-day and 90-day retention rate for users who experienced AI (vs. control)
- Churn rate from AI-related negative experiences

---

## Evaluation Anti-Patterns

**Optimizing model metrics without measuring product impact**: A model with 94% intent accuracy that drives 25% escalation rate has failed as a product. Model metrics are inputs to product success, not the outcome.

**Using test set accuracy as a proxy for production performance**: Test sets are curated; production traffic is not. The gap between them is where most real failures live. Shadow mode evaluation on real traffic is non-negotiable.

**Measuring interaction-level success without downstream metrics**: A customer support AI with 70% containment rate looks successful. Add the 30-day repeat contact metric and you might find 40% of "contained" interactions come back. That's not success — that's deferral.

**Skipping human evaluation for generation tasks**: Automated metrics (BLEU, ROUGE, BERTScore) don't capture whether an AI-generated response is factually correct, appropriate for the context, or useful to the user. Budget for a human evaluation sample at every major product milestone.

**Failing to separate evaluation by intent/segment**: Aggregate accuracy of 90% can mask 60% accuracy on a specific high-volume intent. Always break down evaluation by segment — intent type, user cohort, channel, language — because aggregate metrics hide the failures that actually matter.

---

## Evaluation Infrastructure Checklist

Before shipping any AI feature, verify:

- [ ] Labeled test set exists with >500 examples per major intent/category (for classification)
- [ ] Human evaluation rubric defined with 3+ raters and inter-rater reliability >0.7
- [ ] Shadow mode infrastructure is in place (can run model on real traffic without surfacing output)
- [ ] Go/no-go thresholds documented and signed off by PM, engineering, and compliance
- [ ] Rollback mechanism exists and has been tested
- [ ] Downstream metric tracking is in place (not just session-level — 7-day and 30-day)
- [ ] Model monitoring alerts configured for drift detection
- [ ] Compliance audit trail is logging every AI decision with timestamp, input, output, confidence score

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../README.md)*
