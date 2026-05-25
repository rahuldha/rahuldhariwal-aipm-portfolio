# PRD: AI Evaluation Framework for Conversational AI Products

**Product**: Internal AI Evaluation Platform
**Author**: Rahul Dhariwal
**Status**: Sample / Portfolio
**Date**: May 2026

---

## Problem Statement

Teams building conversational AI products lack a standardized, repeatable framework for evaluating model quality before and after production deployment. The current state: each product team builds ad hoc evaluation tooling, test sets are inconsistently sized and labeled, evaluation metrics are defined differently across teams, and there is no shared infrastructure for shadow mode testing or production monitoring.

The result: teams either ship AI features with insufficient evaluation (resulting in quality incidents in production) or over-invest in evaluation infrastructure (building the same tooling multiple times). Both are waste. The absence of a shared evaluation platform also means leadership has no consistent view of AI quality across the product portfolio.

This PRD defines an internal AI Evaluation Platform — the shared tooling that gives all product teams a standard evaluation workflow without requiring each to build their own.

---

## User Persona

**Primary**: AI PM / ML Engineer on a product team building or maintaining a conversational AI feature. Works on a team of 2-5 ML engineers and 1-2 PMs. Ships AI features on a 6-8 week cycle. Currently manages evaluation tooling manually (custom Python scripts, spreadsheets for human review, Slack for coordination between reviewers).

**Jobs to Be Done**:
1. Know whether my model is ready to ship before I commit to a launch date
2. Know whether my model's performance has degraded after a production change
3. Compare performance across model versions without rebuilding the comparison tooling each time
4. Get sign-off from compliance on AI quality without a manual audit process

**Secondary**: AI Portfolio Lead / Head of AI. Needs a dashboard view of AI quality across all products. Currently unable to answer "which of our AI features has the most quality risk right now?" in under an hour.

---

## Success Metrics

| Metric | Baseline | Target |
|--------|----------|--------|
| Time to evaluate a new AI feature pre-launch | ~3 weeks (custom tooling) | <5 days (using shared platform) |
| % of AI features with a documented evaluation rubric before launch | 40% | 100% |
| Mean time to detect a production quality regression | 7-14 days (manual review) | <48 hours (automated monitoring) |
| Cross-team evaluation reuse rate | ~0% | >60% of new features reuse existing test infrastructure |
| Compliance audit preparation time | 2-3 weeks | <2 days (audit-ready exports built in) |

---

## Requirements

### FR-1: Test Set Management
- Create and version evaluation datasets: conversation logs, intent labels, expected outputs
- Support multiple dataset types: classification test sets (input + label), generation test sets (input + reference output + evaluation rubric), retrieval test sets (query + relevant document labels)
- Minimum required fields: unique ID, input text, expected label/output, metadata (source, date, annotator, language)
- Dataset versioning: immutable once published to prevent retroactive result manipulation; new versions create new dataset IDs
- Import formats: CSV, JSONL, Dialogflow export, direct API from conversation log database

### FR-2: Evaluation Execution
- Run automated evaluations against a defined dataset + model endpoint
- Supported model input/output types: text classification (intent), named entity recognition (slot fill), text generation (response quality), embedding-based retrieval (semantic similarity)
- Built-in metrics: accuracy, precision, recall, F1 (classification); ROUGE, BERTScore, faithfulness (generation); Recall@K, MRR, context precision (retrieval)
- Custom metric support: teams can define evaluation functions in Python and register them as custom metrics
- Evaluation jobs run asynchronously; PM/ML engineer gets Slack notification when complete
- Results stored with: dataset ID, model version, timestamp, full metric breakdown by class/segment

### FR-3: Human Evaluation Workflow
- Create human evaluation tasks with: rating criteria, example ratings (for calibration), scale definition (binary, 1-5, rubric-based)
- Assign to internal annotators or external vendor (vendor API integration required)
- Inter-annotator agreement calculation (Cohen's kappa, Fleiss' kappa)
- Flag items for adjudication when annotators disagree beyond threshold
- Batch size management: auto-split large datasets into batches; track completion per annotator

### FR-4: Shadow Mode Infrastructure
- Route a configurable % of live production traffic to an evaluation endpoint (not shown to users)
- Log: production model output, shadow model output, raw input, metadata
- Compare shadow model vs. production model on: confidence score distribution, output distribution, agreement rate, divergence cases
- Divergence review queue: cases where shadow and production outputs disagree, flagged for human review
- Shadow mode requires explicit PM approval to enable (prevents accidental exposure of experimental models)

### FR-5: Production Monitoring
- Configurable monitoring rules: "alert if intent class distribution shifts >15% from baseline week-over-week"
- Supported metrics: confidence score distribution, class distribution, escalation rate, latency P50/P95/P99
- Alert channels: Slack, email, PagerDuty integration
- Automatic performance snapshot every 7 days for trend analysis
- Anomaly detection: statistical process control charts for early drift detection

### FR-6: Audit & Compliance Exports
- One-click export: evaluation results in CSV/JSON for compliance review
- Audit trail: who ran which evaluation, on which dataset, with which model, when — immutable log
- Compliance attestation: PM can mark an evaluation as the "pre-launch sign-off" evaluation; this creates a dated, signed artifact stored in compliance vault
- Sample export: generate a random stratified sample of production interactions with model outputs for regulatory review

---

## Open Questions

1. **Annotation vendor vs. internal team**: For human evaluation tasks, do we use an external annotation vendor (faster, cheaper, but requires data sharing agreements) or build an internal annotation team? Cost and data privacy implications differ significantly. *Decision needed from: Legal, Finance.*

2. **Self-hosted vs. SaaS**: Should the platform be built internally (full control, but 6+ months to build) or built on a SaaS evaluation tool (Braintrust, LangSmith, Scale AI Eval) with internal customization on top? The SaaS option is faster but creates vendor dependency and has data residency implications for regulated customer data. *Decision needed from: Engineering leadership, Security.*

3. **Centralized vs. federated**: Should evaluation datasets be centralized in a single platform (easier to share and compare across teams) or federated (each team owns its datasets, central platform only handles execution and monitoring)? Centralized is more efficient long-term; federated is faster to adopt. *Decision needed from: PM leads for each product team.*

4. **Access controls for shadow mode**: Who can approve enabling shadow mode on a production feature? Shadow mode routes real customer data to an experimental model — there are data governance implications. Current proposal: PM + Engineering lead approval for each shadow mode instance. *Decision needed from: Security, Compliance.*

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../README.md)*
