# Google / Google Cloud — Company-Specific Interview Prep

**Role target**: Product Manager, Google CCAI / Contact Center AI / Vertex AI / Conversational AI
**Why Google makes sense for my background**: I built the Discover NL IVR on Google CCAI (Dialogflow CX + CCAI Insights) — I have direct hands-on production experience with Google's conversational AI stack. I know where it excels (ASR quality, Dialogflow CX dialog management), where it has gaps (NLU customization overhead, eval tooling), and what enterprise customers need that the current product doesn't deliver.

---

## Company Context

**What Google does (AI surface)**:
- **Google CCAI**: Contact Center AI — Dialogflow CX (conversational AI platform), Insights (call analytics), Agent Assist (human-in-the-loop AI for agents). Used at Discover (my direct experience)
- **Vertex AI**: Google Cloud's AI platform — model training, serving, MLOps, foundation models (Gemini)
- **Gemini**: Google's flagship LLM family — Gemini 1.5 Pro (long context), Gemini 2.0, Ultra/Pro/Flash tiers
- **Agentspace**: Google's enterprise agentic AI platform (2025) — competing with Salesforce Agentforce
- **Google Workspace AI**: Duet AI (now Gemini for Workspace) — AI features in Docs, Sheets, Gmail, Meet

**Revenue**: Google Cloud revenue: $43B (2024), growing ~28% YoY. AI products are the primary growth driver. CCAI is part of Google Cloud's go-to-market but revenue is not separately disclosed.

**Enterprise position**: Google Cloud is #3 in cloud (after AWS and Azure) but is gaining in AI-specific workloads. Gemini 1.5 Pro's 1M token context window is a differentiated capability. CCAI has deep enterprise penetration in regulated industries (financial services, healthcare, telco).

---

## Why I'm a Fit

- **Direct CCAI production experience**: I built on Dialogflow CX for Discover's NL IVR. I know the product from the inside — the dialog state management, entity extractor configuration, fulfillment webhooks, confidence threshold controls, and the handoff to CCAI Insights. This gives me credibility that no other candidate can credibly claim
- **Enterprise conversational AI at scale**: 5M+ annual calls on the Discover NL IVR, 1M+ customers on the Oportun AI agent platform. I've solved the production-scale problems that CCAI enterprise customers face
- **Regulatory industry context**: Google CCAI's primary enterprise segments are financial services, healthcare, and telco — all regulated. My FDCPA/CFPB/FCRA experience maps directly to what CCAI customers need from a compliance and audit trail perspective
- **The gaps I can help close**: Having been a CCAI customer, I know the product gaps — NLU customization requires significant engineering overhead, the eval tooling is limited, and the hand-off between Dialogflow CX and CCAI Insights isn't seamless. I can bring customer-perspective product insight to close these gaps

---

## Questions to Expect

### "What would you build for Google CCAI?"

**My angle**: The biggest gap I experienced as a CCAI customer was **evaluation infrastructure**. Dialogflow CX is a strong dialog management platform but has no native way to run offline evaluations — you can't test "if we change the confidence threshold from 0.75 to 0.70, what happens to our false routing rate?" without running it on live traffic. I built this infrastructure manually at Discover (pulling conversation logs, running simulated classification, modeling error distributions) — it took 3 months of engineering time that should have been zero.

I would build **CCAI Eval Suite**: a native evaluation platform for Dialogflow CX that allows enterprise customers to define test conversation sets, run offline simulations of configuration changes (threshold adjustments, intent updates, entity extractor changes), and get a preview of the impact before deploying to production. This reduces the deployment risk that currently makes enterprise customers cautious about updating live systems.

Secondary priority: **CCAI Intent Analytics** — a dashboard that shows, for every intent, the accuracy metrics, the confidence distribution, the escalation triggers, and the CSAT correlation. Currently, this requires CCAI Insights + BigQuery + a custom dashboard. It should be native.

### "How did you use Google CCAI at Discover and what did you learn?"

**My answer**:

At Discover, I used Dialogflow CX for dialog management and custom NLU on top of Google's ASR. The architecture: Dialogflow CX handled conversation state and routing logic; I built a custom intent taxonomy (38 intents, 4 resolution paths) that layered on top of Dialogflow's default NLU because Discover's domain-specific account terminology required custom entity extractors that general-purpose models didn't handle well.

What worked: ASR quality was excellent — Google's speech recognition, trained on billions of hours, had significantly lower word error rate than alternatives we evaluated. Dialogflow CX's flow builder let us model complex dialog trees without writing code, which accelerated iteration speed. CCAI Insights gave us conversation analytics we couldn't have built from scratch.

What didn't work as well: the NLU customization layer required more engineering overhead than I expected — adding new entity types and training the model on domain-specific examples was slower than the Dialogflow documentation implied. The eval tooling gap was the biggest operational pain point (covered above). And the transition from Dialogflow CX to CCAI Insights wasn't seamless — we lost some conversation metadata in the handoff that required custom logging to recover.

### "How does Google CCAI compete with Amazon Connect, Salesforce Agentforce, and Microsoft Azure AI?"

| Dimension | Google CCAI | Amazon Connect + AI | Salesforce Agentforce | Azure AI |
|-----------|-------------|--------------------|-----------------------|----------|
| ASR quality | Industry-leading | Competitive | Via third-party | Competitive |
| CRM integration | Weak (requires Salesforce connector) | Native with Amazon CRM | Native | Native with Dynamics 365 |
| Enterprise reach | Google Cloud customers | AWS customers | Salesforce customers | Microsoft customers |
| Regulatory tooling | Moderate | Moderate | Strong (financial services) | Strong (regulated industries) |
| LLM integration | Gemini (native) | Claude/GPT-4 (via API) | xLAM/frontier LLMs | GPT-4 (native) |

Google CCAI's clearest advantage is ASR quality and Gemini integration. Its clearest gap is CRM integration — most enterprise contact centers run Salesforce or ServiceNow, and the CCAI → Salesforce integration adds deployment complexity that Amazon Connect (for AWS-native shops) or Salesforce Agentforce (for Salesforce-native shops) don't have.

### "What's Google's competitive advantage in enterprise AI?"

**My answer**: Google's structural advantage is the combination of compute (TPU infrastructure at scale), data (decades of Search, YouTube, and Maps data that informs model quality), and vertical integration (own the hardware, the model, and the cloud platform). The competitive risk: Microsoft's OpenAI partnership gave Azure a model-quality advantage that eroded Google's model differentiation. Gemini 2.0 and the Ultra/Pro/Flash tiering are the response. The enterprise AI moat is ultimately a cloud platform moat — winning Vertex AI wins Gemini access, which drives enterprise AI adoption. CCAI is the beachhead in the contact center vertical.

---

## Research to Do Before the Interview

- Use Dialogflow CX free tier — understand the current UX and limitations
- Read Google CCAI case studies on the Google Cloud website — understand the verticals and use cases they're winning
- Review Gemini API documentation — understand the developer experience of building with Gemini vs. GPT-4/Claude
- Understand Vertex AI's MLOps capabilities — how does Google position Vertex vs. SageMaker and Azure ML?

---

## Questions to Ask the Interviewer

- "What's the biggest thing CCAI enterprise customers struggle with post-deployment — and how is the product team addressing it?"
- "How does the CCAI product team coordinate with the Dialogflow platform team and the Gemini team — three different product orgs with overlapping scope?"
- "Where does Google see CCAI in 3 years — standalone platform or absorbed into a broader enterprise AI platform?"

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../../../README.md)*
