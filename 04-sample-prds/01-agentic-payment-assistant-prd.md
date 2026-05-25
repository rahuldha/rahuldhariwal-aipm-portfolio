# PRD: Agentic Payment Assistant for Fintech Lending

**Product**: AI Agent for Payment Servicing
**Author**: Rahul Dhariwal
**Status**: Sample / Portfolio
**Date**: May 2026

---

## Problem Statement

Customers with outstanding loans contact support primarily for payment-related needs: confirming upcoming payments, modifying payment dates, understanding payment options, and requesting hardship accommodations. These interactions are high-volume, structurally repetitive, and time-sensitive — a customer calling at 11pm before a 6am payment can't wait for business hours. Current IVR systems handle these poorly (legacy touch-tone, high misroute rate), and live agent queues create wait times that correlate with customer anxiety and complaint rates.

The problem is not that AI can't handle these interactions — it's that the AI products deployed to date treat payment servicing as an information retrieval problem (find and read the policy) rather than an action execution problem (understand the customer's situation, check eligibility, take the appropriate action on the account, and confirm resolution).

---

## User Persona

**Primary**: Maria, 34, Oportun borrower. English/Spanish bilingual. Works variable-hours in service industry. Contacts support via SMS or web chat during non-business hours because her schedule doesn't allow daytime calls. Has one active personal loan. Contacts support 3-4x per year for payment-related issues.

**Jobs to Be Done**:
1. Confirm or modify my next payment date without waiting on hold
2. Understand what my options are if I can't make this payment
3. Enroll in a hardship accommodation without repeating my situation to three different agents
4. Get confirmation that my request was actually processed, in writing, immediately

**Pain points with current state**: Long hold times during payment-related stress, having to re-explain situation when transferred, uncertainty about whether the request was actually processed, no channel for resolution outside business hours.

---

## Success Metrics

### Primary Metrics
| Metric | Baseline | Target | Measurement |
|--------|----------|--------|-------------|
| Containment rate (payment intents) | 35% | 65% | % sessions resolved without human escalation |
| Resolution rate | Not tracked | 75% | % sessions with no repeat contact within 30 days |
| Time to resolution | 8 min (agent) | 2 min (AI) | Session duration for equivalent intent types |
| After-hours resolution rate | ~0% | 55% | % of after-hours payment contacts resolved by AI |

### Secondary Metrics
| Metric | Baseline | Target |
|--------|----------|--------|
| CSAT (payment interactions) | 3.2/5 | 3.8/5 |
| Cost per payment-related interaction | $4.50 | $0.80 |
| Hardship enrollment rate among eligible customers | 12% | 22% |

### Guardrail Metrics (must not worsen)
- FDCPA/FCRA compliance audit pass rate: maintain 100%
- Credit-adjacent decision error rate: <2% of automated decisions
- Customer complaint rate attributable to AI: <0.5% of AI-handled interactions

---

## Requirements

### Functional Requirements

**FR-1: Intent Understanding**
- The agent must correctly classify payment-related intents with >92% accuracy across: balance inquiry, next payment date, payment method change, payment date modification, payment amount question, hardship inquiry, hardship enrollment, payment deferral request
- Must handle Spanish-language input with performance parity within 5% of English metrics
- Must handle multi-intent requests ("I want to change my payment date and also ask about deferral options")

**FR-2: Account Context Retrieval**
- The agent must retrieve and accurately present: current balance, next payment date and amount, payment history (last 6 months), enrolled programs (autopay, hardship), payment methods on file
- Account data must be real-time (not cached >5 minutes) for any transactional interaction
- Must display account data in a format appropriate to the channel (SMS: abbreviated; web chat: full)

**FR-3: Action Execution**
- The agent must be able to execute: payment date modification (within policy rules), payment method update, hardship program eligibility check, hardship enrollment (with documented eligibility check), payment deferral (single-cycle, within policy)
- All consequential actions must require explicit confirmation from customer before execution
- All executed actions must generate a confirmation message with transaction ID sent to customer's preferred contact method

**FR-4: Escalation and Handoff**
- Escalation triggers: customer requests human agent, confidence score <0.75 for any consequential action, 3 consecutive turns without resolution progress, detected distress language, any action outside agent's defined scope
- Handoff must preserve full conversation context (customer identity, what was asked, what was attempted, current account state)
- Estimated wait time must be communicated at point of escalation
- Callback scheduling must be offered if wait time >5 minutes

**FR-5: Compliance and Audit**
- Every AI interaction must be logged: timestamp, channel, customer identifier, intents detected, actions taken, confidence scores, escalation reason (if applicable)
- Customer-facing responses referencing account terms or program eligibility must cite the specific policy being applied
- Any interaction involving credit-adjacent decisions (hardship program enrollment, deferral approval) must be flagged for post-hoc human review within 24 hours
- FDCPA-relevant interactions (payment arrangements with delinquent accounts) must be flagged for compliance review

### Non-Functional Requirements

**NF-1: Latency**
- P50 response time: <1.5 seconds from message receipt to response delivery (all channels)
- P95 response time: <3 seconds
- Account data retrieval must not add >500ms to response latency

**NF-2: Availability**
- 99.9% uptime SLA for the AI agent service
- Graceful degradation: if AI service is unavailable, traffic routes to existing IVR/human agent queue with no customer-facing error; customer is not left in a dead state

**NF-3: Scale**
- Must handle 10K concurrent sessions at peak (month-end payment cycles)
- Must maintain performance metrics at 10x current volume without re-architecture

**NF-4: Language**
- English and Spanish at launch
- Architecture must support additional language addition without full re-build

---

## Open Questions

1. **Hardship program autonomous enrollment threshold**: Should the AI enroll customers in hardship programs autonomously for clear-eligibility cases (account in good standing, first-time request, within policy limits)? Or is human review required for all enrollments? This determines whether FR-3 includes enrollment execution or only eligibility determination. *Decision needed from: Legal, Compliance, Operations.*

2. **Payment deferral policy for delinquent accounts**: Customers with accounts 31-60 days past due may request deferrals. FDCPA considerations apply to AI interactions with delinquent accounts. What are the exact policy boundaries for AI vs. human handling in delinquent cohorts? *Decision needed from: Compliance, Collections team.*

3. **Spanish-language model selection**: Does the Spanish-language NLU use the same underlying model with Spanish-language fine-tuning, or a separate model? Performance parity requirement is clear; architecture decision has cost and maintenance implications. *Decision needed from: Engineering.*

4. **Confirmation channel for actions**: Should post-action confirmation be sent via the same channel (SMS if the customer contacted via SMS), or always to the customer's primary contact method on file? Customers with SMS as contact may prefer email confirmation for records. *Decision needed from: UX research.*

5. **30-day repeat contact attribution**: How do we attribute a repeat contact to an AI resolution failure vs. a separate issue the customer had? The methodology for this metric needs to be defined before it can be measured. *Decision needed from: PM + Data team.*

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../README.md)*
