# Discover Financial Services STAR Stories

*Discover: Natural Language IVR using Google CCAI / Dialogflow CX. $7M+ annual savings. Containment lifted from 35% to 65%. 5M+ calls per year in scope.*

---

## Story 1 — The NL IVR Launch (Flagship)

**Situation**: Discover's IVR system was a legacy touch-tone system built in 2009. Customers hated it — the NPS for IVR interactions was -28. The system required customers to navigate 9-level menus, understand banking jargon ("Press 3 for balance transfers"), and repeat themselves when misrouted. Containment rate was 35% — meaning 65% of all IVR calls were eventually transferred to a live agent, at an average cost of $4.50 per transfer.

**Task**: Lead the product definition and launch of a Natural Language IVR that would replace the touch-tone system for the 5M+ annual calls that were eligible for automation. Success criteria defined upfront: containment rate >60%, NPS improvement >20 points, $7M+ net savings over 3 years.

**Action**:
- Ran a build-vs-buy evaluation: assessed Google CCAI (Dialogflow CX + CCAI Insights), Nuance, and a custom NLU build. Decision: Google CCAI for the NLU and dialog management layer; custom intent taxonomy and entity extractors built in-house because Discover's account product vocabulary was too domain-specific for general-purpose models
- Designed the intent taxonomy: 38 intent categories mapped to 4 resolution paths (self-serve IVR, live agent transfer with context, callback scheduling, and digital channel handoff). Prioritized intents by containment potential × call volume — balance inquiries and payment confirmations ranked highest
- Built the evaluation framework before writing a line of dialog: intent accuracy target (>88%), entity extraction precision (>95%), no-speech timeout handling, Spanish language parity (Discover's Spanish-speaking customers represented 18% of call volume), and NPS target by intent category
- Managed the stakeholder conflict: the call center operations team wanted to reduce the confidence threshold to 0.60 (from the 0.75 I specified) to hit containment targets faster. I modeled the cost of false routings at the lower threshold — projected $2.1M in offset costs from repeat calls — and proposed a tiered threshold approach (0.75 for high-stakes routing, 0.60 for low-stakes informational intents). This was approved after presenting to both VP of Operations and VP of Technology together
- Staged rollout: shadow mode for 6 weeks (AI makes decisions, human IVR handles calls, we measure discrepancy), then 10% → 30% → 60% → 100% traffic by intent group

**Result**: 
- Containment rate: 35% → 65% at full deployment
- NPS for IVR interactions: -28 → +12 (40-point improvement, double the target)
- Annual savings: $7.2M in year 1 ($4.50 average cost per transferred call × reduction in transfer volume)
- Spanish-language parity achieved: containment rate within 3% of English-language rate
- The tiered threshold framework was adopted as standard practice for all future AI routing systems at Discover

**Transferable lesson**: The biggest risk in an AI IVR project isn't the model — it's the organizational pressure to optimize the wrong metric. Containment rate without error rate context produces a system that feels high-performing on paper but generates customer hostility. Designing the success framework before the model exists is the most important product decision in a project like this.

---

## Story 2 — The Confidence Threshold Conflict

**Situation**: Eight months into the NL IVR rollout, the call center operations team proposed lowering the intent classification confidence threshold from 0.75 to 0.60. Their reasoning: the current threshold was routing too many calls to agents (containment was at 58%, below the 65% target) and they were under pressure to hit the cost savings commitment.

**Task**: Evaluate the proposal, determine whether it was the right product decision, and navigate the stakeholder dynamic without creating organizational conflict.

**Action**:
- Pulled the error distribution data: at 0.60 threshold, what was the false routing rate by intent category? The aggregate number looked manageable, but the distribution was not — 4 specific intent categories had dramatically elevated error rates at 0.60, and those happened to be the high-stakes intents (payment disputes, account closures, fraud reporting) where incorrect routing caused the longest call recovery times
- Built the cost model: at 0.60 threshold, incremental containment gain = X calls. But each incorrectly routed call generated a repeat call (at $4.50) plus agent recovery time (average 3 minutes of agent cost). Net savings at 0.60 threshold: $2.1M. Net savings at 0.75 threshold: $4.2M
- Proposed the hybrid: apply 0.60 to the 18 low-stakes informational intents (balance inquiry, statement delivery, payment confirmation) where the cost of incorrect routing was low and recovery was fast. Keep 0.75 for the 20 high-stakes intents where routing errors caused escalation and repeat contacts
- Presented jointly to VP of Operations and VP of Technology with the cost model visible — made it a data decision, not an opinion

**Result**: Hybrid threshold approach approved. Containment reached 65% target within 6 weeks of implementation — same target, different path. Net savings in year 1: $5.9M (exceeded the original 3-year target in year 2). The ops team adopted the tiered threshold framework for all subsequent AI routing decisions.

**Transferable lesson**: When a stakeholder pushes for a change that you believe is wrong, the strongest response is a cost model, not a technical argument. Translate the technical risk into the business metric the stakeholder cares about, then propose an alternative that achieves their goal without accepting the risk.

---

## Story 3 — Building the AI ROI Case from Scratch

**Situation**: When I joined the NL IVR project, there was no business case. The initiative had been greenlit based on a vendor demo and an executive's intuition about AI potential. I was inheriting a project with no defined success criteria, no cost model, and no stakeholder alignment on what "success" meant.

**Task**: Build a rigorous ROI framework that would (1) define success criteria before any technology was selected, (2) give the exec team confidence to fund the full project, and (3) establish the measurement infrastructure to track progress after launch.

**Action**:
- Ran a cost baseline study: analyzed 3 months of IVR call data. Variables: call volume by intent (from call recording transcription), average handle time by intent, transfer rate by intent, cost per agent-handled call (loaded FTE cost / calls per day), and CSAT by interaction type
- Built the savings model: projected containment rate improvement by intent tier, multiplied by transfer volume reduction, multiplied by cost per transfer avoidance. Conservative case: 55% containment (25 bps lift), mid case: 65% (30 bps lift), optimistic case: 75% (40 bps lift). Cross-referenced against Google CCAI reference customer benchmarks
- Defined non-financial success criteria: NPS (measured separately for IVR interactions), Spanish-language parity (critical for Discover's customer demographics), and regulatory compliance (every interaction logged, no FDCPA-adjacent issues)
- Got explicit sign-off on the success criteria before project kickoff — milestone gate where the project would be re-evaluated if mid-case projections weren't tracking by month 9

**Result**: The business case funded the full project at the $4.2M implementation budget. At month 9, we were tracking ahead of mid-case projections ($5.9M run-rate savings). The ROI framework I built became the template used for the next two AI investments Discover made in the contact center.

**Transferable lesson**: The ROI framework is a product artifact, not a finance artifact. The PM who builds it controls the success criteria — and success criteria determines what gets funded, what gets prioritized, and what gets shut down. Own this work even when there's a temptation to let finance define it.

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../../README.md)*
