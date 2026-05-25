# OpenAI — Company-Specific Interview Prep

**Role target**: Product Manager, ChatGPT Enterprise / API Products / Agentic AI
**Why OpenAI makes sense for my background**: OpenAI ships the models and products that define the AI industry category. I've built on GPT-4 in production (Oportun's initial AI agent stack), understand the API from an enterprise deployment perspective, and have the fintech background to contribute to OpenAI's growing enterprise segment.

---

## Company Context

**What OpenAI does**: AI research lab turned commercial AI company. Products: ChatGPT (consumer + Pro + Team + Enterprise), OpenAI API (developer access), Sora (video generation), and the emerging Operator/Agent tier.

**OpenAI's product surface**:
- **ChatGPT**: 300M+ weekly active users. Consumer ($20/month Pro), Team ($25/user/month), Enterprise (custom pricing). GPT-4.1, GPT-4o, o3 (reasoning) models
- **OpenAI API**: the developer platform — the same API layer powering most enterprise AI products
- **Operator / Agents API**: OpenAI's agentic tier — computer use, multi-step task execution, autonomous workflows
- **Custom GPTs**: user-created specialized versions of ChatGPT
- **OpenAI for Enterprise**: Dedicated infrastructure, compliance controls, custom models, SSO

**Revenue**: $3.4B ARR (2024) → $6B+ ARR (2025 estimates). Growing rapidly from enterprise tier and API adoption.

**Culture**: Research-driven, extremely fast-moving, some organizational tension between research priorities and commercial roadmap. Post-Sam Altman reinstatement, commercial scale is the explicit priority.

---

## Why I'm a Fit

- **API-side production experience**: I've run GPT-4 at production scale in regulated fintech — I know the friction points, the failure modes, and the operational overhead of building enterprise AI on OpenAI's stack
- **Enterprise buyer perspective**: I've been on the buying side. I can articulate what enterprise buyers need that OpenAI's current products don't deliver — and prioritize improvements from a customer success perspective
- **Agentic systems experience**: OpenAI's Operator/Agents tier is where the next product chapter is. I built production agentic systems at Oportun — the trust, evaluation, and human-in-the-loop design problems I solved there are exactly what OpenAI's agentic products need to solve at scale
- **Cost modeling at scale**: At Oportun with 1M+ customers, GPT-4 cost was a material line item. I've done the model cost optimization work (model right-sizing, caching, batching) that enterprise customers need and that OpenAI should be helping them do

---

## Questions to Expect

### "What would you build at OpenAI?"

**My angle**: OpenAI's biggest enterprise churn risk is prompt brittleness — a customer builds a product on GPT-4, OpenAI ships GPT-4.1, model behavior changes subtly, the customer's product breaks. I would build **OpenAI Eval Suite**: a native evaluation platform for enterprise API customers that lets them define test cases for their specific product use cases, run automated regressions before and after model version updates, and gate model version adoption to their production traffic. 

This exists as third-party tools (Braintrust, LangSmith) but not natively from OpenAI. Shipping it natively would: reduce churn from model-upgrade breakage, deepen the enterprise relationship, and give OpenAI visibility into how customers are actually using the models — a strategic data asset for roadmap prioritization.

### "How do you think about OpenAI's competitive moat?"

**My answer**: OpenAI's moat is three-layered. Brand/trust (enterprise procurement is easier when buyers are familiar with ChatGPT; it reduces the "AI is scary" objection), data flywheel (300M+ weekly active users generating preference signal that improves model alignment at a scale no competitor can match), and API ecosystem depth (GPT-4 is embedded in thousands of enterprise products — switching cost for those builders is high). The vulnerability: Claude and Gemini are competitive on model quality in most benchmarks; if the ecosystem lock-in erodes, the moat thins. OpenAI's investment in the Operator/agentic tier is the right response — agentic use cases generate deeper product integration and higher switching cost than pure API usage.

### "How would you improve ChatGPT Enterprise?"

**Priority list**:

1. **Cross-conversation memory scoped by team**: Enterprise users want Claude/ChatGPT to remember context across sessions, but enterprise customers don't want AI trained on one employee's conversations influencing another's. Scoped memory (per-user, per-team, per-workspace) with explicit controls is the product gap
2. **Native analytics**: Enterprise admins can't see which teams use AI, for what tasks, with what quality outcomes. Usage analytics with model-agnostic quality scoring would accelerate internal AI adoption by giving admins the data to expand licenses
3. **Custom model tuning without fine-tuning overhead**: Most enterprise customers want a consistent persona and output format, not custom model training. A structured system prompt management layer with version control, testing, and rollout controls achieves 80% of what fine-tuning offers at 10% of the cost and complexity

### "Tell me about a time you had to make a product decision under uncertainty."

*Use the Oportun hardship program launch story from the STAR bank — it maps directly to the AI decision-making under uncertainty frame.*

---

## Research to Do Before the Interview

- Use GPT-4.1 and o3 extensively — understand the qualitative difference in how they handle reasoning-heavy tasks
- Read OpenAI's API documentation for Assistants API, Batch API, and the Agents SDK — understand the developer experience
- Review OpenAI's enterprise case studies (available on their website) — understand what use cases they're winning in enterprise
- Understand OpenAI's model versioning policy — how long do deprecated models stay available? This is a pain point for enterprise customers

---

## Questions to Ask the Interviewer

- "How does OpenAI's research roadmap interact with the commercial product roadmap — who wins when they conflict?"
- "What's the biggest operational challenge for enterprise customers that OpenAI hasn't fully solved yet?"
- "Where is ChatGPT Enterprise underperforming against the market expectation right now, and what's blocking progress?"

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../../../README.md)*
