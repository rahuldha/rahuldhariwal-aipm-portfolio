# AI Feature Prioritization Framework

*Standard RICE doesn't work for AI features — it ignores the two factors that most often kill AI projects: data readiness and failure mode risk. This is the modified scoring framework I use.*

---

## The Framework: RICE + DR (Data Readiness + Risk)

### Standard RICE
- **R** — Reach: how many users affected per quarter
- **I** — Impact: 0.25 (minimal) / 0.5 (low) / 1 (medium) / 2 (high) / 3 (massive)
- **C** — Confidence: % confidence in estimates (typically 50/80/100%)
- **E** — Effort: person-weeks

**Standard RICE Score** = (R × I × C) / E

### AI Modifiers

**Data Readiness (DR)**: multiplier on the standard RICE score — 0.3 to 1.0
| Score | Condition |
|-------|-----------|
| 1.0 | Labeled training data exists, volume is sufficient (>10K examples for classification), signal quality is validated |
| 0.7 | Unlabeled data exists and can be labeled at reasonable cost; baseline model performance is testable |
| 0.5 | Data exists but quality is uncertain; significant data cleaning or collection required |
| 0.3 | Data is sparse, proprietary to third parties, or requires a new data collection pipeline |

**Failure Mode Risk (FMR)**: discount factor — 0.5 to 1.0
| Score | Condition |
|-------|-----------|
| 1.0 | Model errors are low-cost, reversible, and easily detectable (user can correct the AI) |
| 0.8 | Model errors have moderate impact but are detectable and recoverable in the same session |
| 0.6 | Model errors have significant downstream cost (customer complaint, repeat contact, wasted work) but no regulatory exposure |
| 0.5 | Model errors create regulatory risk, legal exposure, or irreversible user harm |

### Modified Score
**AI RICE Score** = (R × I × C / E) × DR × FMR

---

## How to Use It

### Step 1: Score each candidate feature on standard RICE
Run a standard RICE scoring exercise with your team. Get rough estimates — the point is relative ranking, not precision.

### Step 2: Apply the AI modifiers
For each feature, answer two questions:
1. Do we have the data to actually build this? (Data Readiness)
2. What happens when the model is wrong? (Failure Mode Risk)

Features that score high on standard RICE but low on DR or FMR should be **staged** — not dropped from the roadmap, but broken into two phases: Phase 1 builds the data infrastructure or de-risks the failure mode, Phase 2 builds the AI feature.

### Step 3: Categorize outputs
After scoring, features fall into four categories:

| Category | High DR | Low DR |
|----------|---------|--------|
| **High FMR** | Build now — data ready, failure is manageable | Data-first — collect data before committing to AI |
| **Low FMR** | Guardrail-first — build the human-in-the-loop before scaling | Defer — both data and failure mode need work |

---

## Example: Oportun AI Agent Prioritization

| Feature | R | I | C | E | RICE | DR | FMR | AI Score |
|---------|---|---|---|---|------|----|-----|----------|
| Balance/payment info (informational) | 500K | 2 | 80% | 4 | 200K | 1.0 | 1.0 | 200K |
| Payment arrangement modification | 200K | 3 | 80% | 8 | 60K | 0.7 | 0.7 | 29.4K |
| Hardship program enrollment | 80K | 3 | 60% | 10 | 14.4K | 0.5 | 0.5 | 3.6K |

**Decision**: Launch informational intents first (high data readiness, low failure risk). Stage payment modification with data collection sprint in Q1, AI build in Q2. Hardship enrollment requires human-in-the-loop design before autonomous AI handling.

This is the actual sequencing we used — not built on instinct, built on the scoring.

---

## Common Mistakes

**Skipping data readiness assessment**: Teams commit to AI features without validating they have training data. Discover this 3 months in and you've lost a quarter.

**Treating all model errors as equal**: A chatbot suggesting the wrong lunch restaurant is not the same as an IVR routing a dispute to the wrong queue. FMR scoring forces the team to think about failure mode severity before not after shipping.

**Optimizing the AI score to justify a pet feature**: If your DR and FMR scores are consistently 1.0, you're not applying the framework — you're rationalizing decisions you've already made.

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../README.md)*
