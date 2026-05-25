# Product Strategy — AI PM Question Bank

*How I approach product strategy questions in AI PM interviews: ground every answer in a specific user problem, quantify the business impact, and show you understand where AI adds unique value vs. where it adds risk.*

---

## Q: How do you decide which problems to solve with AI vs. traditional software?

**Framework**: AI is the right tool when the problem has three properties — the input space is too large for rules, the output is probabilistic rather than deterministic, and there's enough data to learn from. Traditional software is better when logic is enumerable, the stakes of errors are high and non-correctable, or the problem is fully solved by business rules.

**My answer**: At Oportun, I applied this test before each AI feature decision. Payment arrangement negotiation was a strong AI candidate: thousands of distinct customer situations, no single "right" answer, and 3 years of agent conversation history to learn from. Account verification, by contrast, stayed rules-based — the logic was enumerable, the error cost was regulatory, and ML variability added risk without adding value.

**Key trade-off to surface**: AI introduces latency, cost, and non-determinism. If the problem doesn't require contextual judgment at scale, traditional software has better reliability and explainability at lower operational overhead.

---

## Q: How do you prioritize features in an AI product roadmap?

**Framework**: RICE scoring as baseline (Reach × Impact × Confidence ÷ Effort), with two AI-specific modifiers — Data Availability (can we actually build this given the training signal we have?) and Failure Mode Risk (what happens when the model is wrong, and how often?).

**My answer**: At Oportun, I ran quarterly roadmap reviews with a modified RICE that added a "model readiness" score — 0-3 based on labeled data volume, signal quality, and baseline model performance on held-out test data. Features with high RICE but low model readiness were staged: design sprint now, build data collection infrastructure, schedule model development 2 quarters out. This prevented the failure mode of shipping a product where the AI was cosmetic rather than capable.

**Key trade-off to surface**: Standard RICE underweights evaluation infrastructure investment. I always add an evaluation rubric to the roadmap as a first-class deliverable — the cost of building a feature without a quality measurement framework is compounded every quarter you can't tell if the feature is working.

---

## Q: How do you think about build vs. buy for AI capabilities?

**Framework**: Buy when the capability is undifferentiated, when the vendor has orders-of-magnitude more training data than you can assemble, and when switching cost is low. Build when the capability depends on proprietary data, when the model's behavior is load-bearing for the user experience, or when regulatory requirements demand explainability and auditability that vendor black boxes can't provide.

**My answer**: At Discover, I evaluated Google CCAI vs. building a custom NLU. The decision was buy — Google's ASR had 500M+ hours of training data we couldn't replicate, and Dialogflow CX gave us the dialog management framework to iterate. But I scoped what we'd build on top: custom intent taxonomy (our IVR had domain-specific language no general model handled), custom entity extractors for Discover-specific account types, and a proprietary confidence threshold framework. The split was: buy the foundation, build the differentiation.

**Key trade-off to surface**: Vendor dependency risk scales with how embedded the vendor's model is in your product's core loop. If you can't swap the vendor without rebuilding the product, the "buy" decision needs a clear exit strategy or contractual protections on model performance and data portability.

---

## Q: How would you approach launching a new AI feature for a regulated industry (fintech, healthcare)?

**Framework**: The regulatory constraint isn't just a compliance checkbox — it shapes the product architecture. For regulated industries: (1) define the AI's decision boundary precisely (what decisions does it make vs. recommend), (2) build explainability into the output (why did the model do that?), (3) design human override at every consequential action, (4) establish audit trail from day one, and (5) stage rollout with go/no-go criteria tied to error rates, not just user adoption.

**My answer**: At Oportun, every AI agent decision that touched account actions (payment arrangements, forbearance, hardship programs) required a human escalation path and an explainability log. I built the evaluation framework before we wrote the first line of model code — if we couldn't explain why the model recommended X, we couldn't ship it. This added 6 weeks to the first sprint but prevented a compliance incident 8 months later when the CFPB requested a sample of AI-assisted customer interactions and we could produce a full audit trail.

**Key trade-off to surface**: Explainability often trades off against model performance. Logistic regression is explainable; transformer-based models are accurate. For regulated industries, I start with an explainable baseline and measure how much performance you're leaving on the table — that gap is the ROI calculation for the regulatory and engineering investment to make the complex model explainable.

---

## Q: How do you measure success for an AI feature?

**Framework**: Three layers — model metrics (is the AI performing?), product metrics (is the feature driving the intended user behavior?), and business metrics (is it moving the business outcome?). You need all three; any single layer misleads.

**My answer**: At Oportun, the AI agent had:
- **Model layer**: intent classification accuracy (>92% threshold), slot fill precision (>95%), confidence score calibration
- **Product layer**: containment rate (% resolved without human escalation), CSAT for AI-handled vs. human-handled interactions, median resolution time
- **Business layer**: cost per interaction (AI vs. human), repeat contact rate within 30 days (quality signal), regulatory compliance audit pass rate

The key insight: containment rate (product metric) looked great at 68%, but 30-day repeat contact rate (business metric) was elevated for 3 specific intent categories — meaning we were "resolving" in the technical sense but not solving the customer's actual problem. We routed those intents back to human agents and restarted the training loop with labeled escalation data.

**Key trade-off to surface**: Model metrics and product metrics can be inversely correlated — a model that always escalates to humans has 0% model error rate but a useless product. Set success criteria at the business metric layer first, then derive the model and product metrics that predict it.

---

## Q: Tell me about a product you'd build at this company.

*[Customize per company — see company-specific prep files. General framework below.]*

**Framework**: Structure the answer as a mini-PRD — problem, user, insight, solution, success metric. Show that you've done research on the company's actual product gaps, not just generic AI ideas.

**What makes an answer strong**: Specificity about the user problem (a real pain point, not a made-up one), an understanding of why now (what's changed technically or in the market that makes this buildable?), and a clear success metric that ties back to the company's business model.

---

## Q: How do you handle model degradation in production?

**Framework**: Degradation has three causes — data drift (input distribution shifted), concept drift (the relationship between input and output changed), or infrastructure failure. Each requires a different response. The defense is a monitoring stack that catches each type before it affects users at scale.

**My answer**: At Oportun, I built a monitoring framework with three signals: (1) prediction distribution shift (if intent class probabilities shift more than 15% week-over-week, alert), (2) ground truth comparison on a sampled 2% of interactions with human review, and (3) downstream business metrics (containment rate, CSAT) as lagging indicators. The key design decision: the first two are real-time automated checks; the third requires human investigation. We set go/no-go thresholds that paused AI handling and routed to human agents if any metric crossed a defined threshold — this happened once, during a major policy change when our knowledge base update lagged behind the call center training by 72 hours.

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../../README.md)*
