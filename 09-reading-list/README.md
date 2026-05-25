# Reading List

*Annotated resources that shaped my AI PM thinking. Each entry includes what it is, why it matters, and what I took from it as a practitioner.*

---

## Foundational AI / ML Concepts

**"Attention Is All You Need" (Vaswani et al., 2017)**
The paper that introduced the Transformer architecture — the foundation of every modern LLM. You don't need to understand the math, but understanding that attention mechanisms let models weigh the relevance of every word against every other word in the input changes how you think about context window design and why long-context performance degrades.
*What I took from it*: The architecture is why LLMs can reason about relationships across long documents — and why they lose performance at context edges. Directly informed how I structured knowledge base chunking at Oportun.

**"BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding" (Devlin et al., 2019)**
The paper that established the pre-train / fine-tune paradigm for NLP. Understanding this paper explains why fine-tuning on domain-specific data works (and why it's expensive).
*What I took from it*: The pre-train / fine-tune distinction is the mental model that resolves most "should we build or buy our NLU?" debates.

**"Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" (Lewis et al., 2020)**
The original RAG paper from Meta AI. Introduces the architecture that grounds LLMs in external knowledge bases.
*What I took from it*: The fundamental insight — don't bake knowledge into model weights when you can retrieve it — is the single most important architectural pattern for enterprise AI products. It's why Notion AI, Intercom Fin, and every enterprise support AI works the way it does.

---

## Agentic AI

**"ReAct: Synergizing Reasoning and Acting in Language Models" (Yao et al., 2022)**
The paper that introduced the ReAct prompting pattern — LLMs alternating between reasoning steps and action steps. This is the conceptual foundation of how modern AI agents work.
*What I took from it*: The reason agentic systems fail is often the reasoning step, not the action step. Designing the reasoning trace is a product design problem, not just an engineering problem.

**"Toolformer: Language Models Can Teach Themselves to Use Tools" (Schick et al., 2023)**
How LLMs learn to call external tools (calculators, search APIs, code execution). Relevant to any product building an AI agent with API integrations.
*What I took from it*: Tool calling is learnable — but the tool definitions (how you describe what a tool does) dramatically affect whether the model uses them correctly. Tool definition design is a product responsibility.

---

## Evaluation & Measurement

**"HELM: Holistic Evaluation of Language Models" (Liang et al., 2022)**
Stanford's framework for evaluating LLMs across 7 metrics (accuracy, calibration, robustness, fairness, bias, toxicity, efficiency) and 42 scenarios. The most rigorous public benchmark framework.
*What I took from it*: Evaluation frameworks need to be multi-dimensional. Any single metric (accuracy, BLEU, perplexity) is gameable and misleading. I built my three-layer evaluation structure (model / product / business metrics) after reading HELM.

**"Measuring Massive Multitask Language Understanding" (Hendrycks et al., 2021)**
The MMLU benchmark — tests LLMs across 57 academic subjects. Useful for understanding what "model quality" benchmarks actually measure (and don't measure).
*What I took from it*: Benchmark scores don't predict product performance on your specific use case. Always validate on your domain's actual input distribution.

---

## AI Product Strategy

**"The Bitter Lesson" (Richard Sutton, 2019)**
A short essay arguing that general methods that leverage computation always outperform methods that leverage domain knowledge over the long run. Written by one of the founders of reinforcement learning.
*What I took from it*: The reason foundation models beat carefully engineered rule-based systems is computational scale, not clever architecture. This is the strategic argument for why "build a large model" often beats "build a better expert system."

**"On the Opportunities and Risks of Foundation Models" (Bommasani et al., 2021)**
Stanford's 200-page report on foundation models — what they can do, where they fail, and what the research and policy community needs to address.
*What I took from it*: The homogenization risk. When everyone builds on the same foundation models, failures in those models propagate across thousands of products simultaneously. Building on multiple foundation models (not being OpenAI-exclusive) is a resilience strategy, not just a hedging strategy.

**"AI Product Management" (Teresa Torres approach + AI-specific adaptations)**
No single source — synthesized from Teresa Torres's continuous discovery habits framework + AI-specific adaptations. The key adaptation: outcome-based roadmaps become harder with AI because AI outcomes are probabilistic, not deterministic.
*What I took from it*: Define the outcome you want to change, not the AI feature you want to ship. "Increase containment rate by 20%" is an outcome. "Ship an AI chatbot" is not.

---

## Conversational AI

**Google CCAI / Dialogflow CX Documentation**
Not a paper — the production documentation I lived in during the Discover NL IVR build. The intent taxonomy design, entity extraction configuration, confidence threshold behavior, and fulfillment webhook architecture are all documented here.
*What I took from it*: Production conversational AI is 80% data pipeline and 20% model. The Dialogflow documentation's emphasis on training phrase diversity, entity synonym coverage, and confidence threshold calibration maps directly to real-world NLU quality.

**"Natural Language Processing with Transformers" (Tunstall, von Werra, Wolf — O'Reilly 2022)**
The most practical book for PMs who want to understand NLP deeply enough to make informed decisions. Written by Hugging Face researchers. No PhD required.
*What I took from it*: The chapters on text classification and token classification directly informed how I designed the intent taxonomy and entity extraction framework at Discover. Understanding how the model represents multi-class classification problems changed how I structured the training data.

---

## Business & Market Context

**"The AI Product Playbook" (various Lenny's Newsletter / a16z publications)**
Synthesized from multiple sources: Lenny Rachitsky's AI PM interviews, Andreessen Horowitz AI product market maps, and Sequoia's AI market frameworks.
*What I took from it*: The "wrapper vs. platform vs. model" positioning debate. Products that wrap frontier models without adding proprietary value (data, workflow integration, distribution) are commoditized when model providers add those features natively. The durable layer is proprietary data + workflow integration, not the AI capability itself.

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../README.md)*
