# PM Prompt Templates

*Production-tested prompts for AI PM workflows. Each template includes the context you need to set, the task to delegate, and the output format to request.*

---

## Product Research & Analysis

### Competitive Analysis Prompt
```
You are a senior product strategy analyst. I need a competitive analysis of [PRODUCT/FEATURE] in the [MARKET SEGMENT] space.

Structure your analysis as:
1. Market Size & Growth: TAM, SAM, key growth drivers
2. Key Players: top 3-5 competitors, their positioning, and differentiation
3. Feature Matrix: compare [list 5-8 key capabilities] across competitors in a table
4. Competitive Dynamics: who is gaining/losing share, and why
5. Gaps & Opportunities: underserved needs or positioning gaps a new entrant could exploit
6. Open Questions: what we still don't know that matters

Be specific. Use named companies, actual metrics where available, and cite whether numbers are from public sources or estimates. Avoid generic statements like "AI is transforming the industry."

Market/product context: [INSERT CONTEXT]
```

### User Interview Synthesis Prompt
```
I have [N] user interview transcripts about [TOPIC/PRODUCT AREA]. Synthesize the findings into:

1. Top 3 Jobs to Be Done (JTBD): what are users actually trying to accomplish? Use the format "When [situation], I want to [motivation], so I can [outcome]"
2. Pain Points by Frequency: ranked list of frustrations, with a count of how many users mentioned each
3. Current Workarounds: what are users doing instead of using the ideal solution?
4. Surprising Findings: what was unexpected or contradicts our current assumptions?
5. Open Questions: what do we still not understand?

Transcripts:
[PASTE TRANSCRIPTS]
```

### PRD Review Prompt
```
Review the following PRD as a skeptical senior PM. Your job is to find gaps, not validate my thinking.

Specifically, assess:
1. Problem Clarity: Is the problem statement specific enough to test? Could we measure whether we've solved it?
2. User Persona: Is the persona specific enough to make design decisions? What's missing?
3. Success Metrics: Are the primary metrics actually measuring whether the problem is solved, or just whether the feature was used?
4. Requirements Gaps: What edge cases, error states, or failure modes are not addressed?
5. Open Questions: Which of the listed open questions are most critical to resolve before development starts?
6. What's Missing: What should be in this PRD that isn't?

PRD:
[PASTE PRD]
```

---

## AI Feature Design

### AI Failure Mode Analysis Prompt
```
I am designing [AI FEATURE DESCRIPTION]. Help me identify the failure modes before we build it.

For each failure mode, provide:
- What the failure looks like from the user's perspective
- How frequently you'd expect it to occur (rare / occasional / common)
- The severity if it occurs (low / medium / high / critical)
- What mitigation exists in the current design
- What additional mitigation I should consider

Categories to analyze:
1. Model quality failures (wrong outputs, hallucination, low confidence)
2. Data failures (stale information, missing context, retrieval failures)
3. Edge cases in user input (unusual phrasing, multi-intent, adversarial input)
4. System failures (latency, downtime, third-party API failures)
5. Compliance/safety failures (regulatory violations, harmful outputs, privacy issues)

AI Feature: [DESCRIPTION]
User context: [WHO USES IT, IN WHAT CONTEXT]
Constraints: [ANY KNOWN REGULATORY, SAFETY, OR BUSINESS CONSTRAINTS]
```

### Evaluation Rubric Generator Prompt
```
I am building [AI FEATURE TYPE] for [USE CASE]. Help me define the evaluation framework.

Generate:
1. Model Metrics: the specific metrics I should use to evaluate model quality (with target thresholds for a production-ready bar)
2. Product Metrics: the user behavior metrics that indicate the feature is working as intended
3. Business Metrics: the downstream business outcomes this feature should drive
4. Guardrail Metrics: the metrics that must not worsen (even if primary metrics improve)
5. Human Evaluation Rubric: if human review is needed, what criteria should raters use? (provide a 1-5 scale with descriptions for each score)

For each metric, specify: how to measure it, how often to measure it, and what the go/no-go threshold should be for a new feature launch.

Feature: [DESCRIPTION]
Current baseline (if known): [CURRENT PERFORMANCE]
```

### Build vs. Buy Analysis Prompt
```
I need to decide whether to build or buy [AI CAPABILITY] for [PRODUCT CONTEXT].

Analyze the decision across these dimensions:
1. Strategic Control: how important is ownership of this capability for long-term differentiation?
2. Data Advantage: do we have proprietary data that would make a custom-built model outperform a vendor solution?
3. Vendor Landscape: what are the top 3 vendors for this capability? What are their pricing models, limitations, and lock-in risks?
4. Build Cost: what would it realistically take to build this in-house? (team, time, ongoing maintenance)
5. Buy Cost: total cost of ownership for the vendor option (licensing, integration, switching cost)
6. Recommendation: given the above, what's the recommended path? What would change the answer?

Context:
- Capability needed: [DESCRIBE]
- Our current tech stack: [RELEVANT STACK]
- Team: [PM + ENGINEERING CAPACITY]
- Regulatory requirements: [IF ANY]
- Timeline: [WHEN DO WE NEED THIS]
```

---

## Communication & Stakeholder Alignment

### Executive Summary Prompt
```
I need to summarize [AI INITIATIVE / DECISION] for a C-suite audience who will read this in under 2 minutes.

Write a 3-paragraph executive summary:
Paragraph 1: The problem we're solving and why it matters now (business impact)
Paragraph 2: What we're doing about it (approach, not technical detail)
Paragraph 3: What success looks like and what we need from leadership (specific ask)

Rules:
- No jargon (no "LLM", "RAG", "embeddings" — translate to business terms)
- Lead with the business outcome, not the technology
- Include one specific metric per paragraph
- End with a clear ask (approval, resource, decision)

Initiative context: [DESCRIPTION]
Key metrics: [RELEVANT NUMBERS]
Ask: [WHAT DO YOU NEED FROM LEADERSHIP]
```

### AI Incident Communication Prompt
```
We have an AI quality incident that needs to be communicated to [AUDIENCE: customers / internal stakeholders / leadership].

Write the incident communication with:
1. What happened (plain language, no jargon)
2. Who was affected and how
3. What we did to fix it (or are doing)
4. What we're doing to prevent recurrence
5. What affected users should do (if anything)

Tone: [TRANSPARENT AND DIRECT / REASSURING / FORMAL]
Audience: [CUSTOMERS / INTERNAL / LEADERSHIP]
Facts of the incident: [DESCRIBE WHAT HAPPENED, DURATION, SCOPE]
```

---

## Metrics & Reporting

### AI Product Health Dashboard Prompt
```
I need to define the metrics dashboard for [AI PRODUCT NAME].

Design a three-tier monitoring framework:
Tier 1 (Real-time alerts — check every 15 min): metrics that indicate a system is failing and need immediate action
Tier 2 (Daily review — morning review): metrics that indicate quality trends and need daily attention
Tier 3 (Weekly/monthly review): metrics that indicate long-term product health and business impact

For each metric:
- Name and definition
- How to calculate it
- Alert threshold (Tier 1 only)
- Who is responsible for investigating when it moves

Product context: [DESCRIBE THE AI PRODUCT, ITS USE CASE, AND ITS USERS]
Current data available: [WHAT LOGS/ANALYTICS DO WE HAVE ACCESS TO]
```

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../README.md)*
