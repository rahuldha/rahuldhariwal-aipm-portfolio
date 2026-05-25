# AI Rollout Framework

*Phased rollout for AI features is structurally different from standard feature rollout. The model's behavior at 1% traffic may not predict behavior at 100% — distribution shift, edge cases at scale, and feedback loop dynamics all emerge only with volume. This is the framework I used at Oportun and Discover.*

---

## The Phases

### Phase 0: Shadow Mode (0% live traffic)
**What it is**: The AI model runs in parallel with the existing system. Real inputs, real context — but the AI's output is logged, not shown to users. The existing system handles all interactions.

**Duration**: 4-8 weeks for a new intent or capability.

**What to measure**:
- Intent classification accuracy vs. ground truth (agent decisions as proxy)
- Slot fill precision on real input data
- Confidence score distribution — is the model calibrated or consistently over/under-confident?
- Edge case identification — what inputs does the model mishandle that test data didn't cover?

**Go/no-go gate**: Intent accuracy >88% on shadow traffic (not test set), confidence calibration within 10% of expected, no systematic failure patterns on high-volume input types.

**Why this phase matters**: Shadow mode catches distribution shift — cases where real user language differs from training data. Test sets are curated; real traffic is not. The gap between them is where most pre-launch surprises hide.

---

### Phase 1: Controlled Launch (1-5% traffic)
**What it is**: AI handles a small fraction of real interactions. Human agents handle the remainder. Rigorous monitoring, daily review.

**Duration**: 2-4 weeks.

**What to measure**:
- Containment rate: % of AI-handled interactions that don't escalate to human
- CSAT for AI-handled interactions vs. human-handled (matched by intent)
- Error rate by intent category
- Escalation triggers: what causes handoff? Expected (low confidence) vs. unexpected (user confusion, rage indicators)
- Downstream metrics: 30-day repeat contact rate for AI-resolved interactions

**Go/no-go gate**: Containment rate within 5% of human baseline for matched intents, CSAT not more than 5 points below human baseline, no regulatory/compliance flags.

**Who reviews**: PM reviews daily summary. Operations team reviews sampled conversations (10-20 per day). Compliance reviews any interaction flagged by confidence anomaly detector.

---

### Phase 2: Expanded Launch (10-25% traffic)
**What it is**: AI handling increases to validate performance holds at higher volume. More edge cases surface.

**Duration**: 4-6 weeks.

**What to measure**: Same as Phase 1, plus:
- Statistical significance check — is the performance difference from human baseline statistically significant, or is Phase 1 data within noise?
- Volume-sensitive failure modes — errors that only appear at scale (specific time-of-day patterns, channel-specific edge cases, load-related latency issues)
- Feedback loop health — is the model's confidence calibration drifting as it handles more traffic?

**Go/no-go gate**: All Phase 1 criteria hold at statistical significance. No new systematic failure modes identified. Latency within SLA at 25x Phase 1 volume.

---

### Phase 3: Full Rollout (40-100% traffic)
**What it is**: AI takes full production traffic for the scoped intents.

**Duration**: Ongoing, with monthly reviews.

**Monitoring cadence**:
- Real-time: latency, error rate, escalation rate — automated alerts if any cross threshold
- Weekly: intent-level performance review, CSAT comparison, cost per interaction
- Monthly: full metrics review, model drift assessment, roadmap update

**Rollback criteria** (hard thresholds that trigger automatic traffic revert):
- Error rate increases >15% week-over-week
- Containment rate drops >10% from established baseline
- CSAT drops >8 points from human baseline
- Any compliance flag from the automated audit checker

---

## Go/No-Go Scorecard Template

| Metric | Threshold | Shadow | Phase 1 | Phase 2 | Status |
|--------|-----------|--------|---------|---------|--------|
| Intent accuracy | >88% | | | | |
| Containment rate | Within 5% of human baseline | | | | |
| CSAT delta | ≤5 points below human | | | | |
| 30-day repeat contact | ≤110% of human baseline | | | | |
| Compliance flags | 0 | | | | |
| Latency P95 | ≤[SLA target] | | | | |
| Confidence calibration | Within 10% | | | | |

Fill this in for each phase. Document the data, not the opinion.

---

## Hard Rules

**Never skip shadow mode.** The temptation is real, especially under deadline pressure. Shadow mode is the most cost-effective insurance against a bad launch — it costs one sprint of engineering but protects months of trust recovery.

**Define rollback criteria before launch, not after.** If you're defining what a bad outcome looks like after you've shipped, you'll always rationalize that the current numbers aren't quite bad enough to roll back. Write the thresholds down and stick to them.

**Measure downstream metrics, not just interaction-level metrics.** A model that answers questions quickly and "correctly" but generates repeat contacts has failed. Add the 7-day and 30-day no-repeat-contact rate to every intent's measurement framework from Phase 1.

**Stage by intent, not by traffic.** Rolling out all intents to 5% traffic simultaneously makes it impossible to isolate which intent is causing problems. Stage by intent group: informational intents first, transactional intents second, complex/high-stakes intents last.

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../README.md)*
