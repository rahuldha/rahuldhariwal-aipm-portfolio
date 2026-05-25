# Technical AI — PM Question Bank

*The goal isn't to answer like an engineer — it's to demonstrate that I can make informed trade-off decisions, set the right success criteria, and communicate with technical teams as a peer, not a translator.*

---

## Q: Explain RAG (Retrieval-Augmented Generation) and when you'd use it.

**The answer**: RAG is an architecture where, instead of relying purely on what an LLM learned during training, you retrieve relevant documents from an external knowledge base at inference time and inject them into the model's context before generating a response. The model answers based on what it was trained on PLUS the specific retrieved context.

**When to use it**: When answers need to be grounded in specific, current, or proprietary knowledge that the base model doesn't have — product documentation, internal policies, customer account data, recent events. Contrast with fine-tuning, which bakes knowledge into model weights and can't be updated without retraining.

**The PM-level trade-off**: RAG adds latency (retrieval takes time), retrieval quality determines answer quality (garbage in, garbage out), and the context window limits how much retrieved content you can inject. Fine-tuning is cheaper at inference time and has no retrieval latency, but updating it requires retraining — expensive and slow for frequently changing knowledge.

**My experience**: At Oportun, I built an agentic RAG system where the knowledge base was our product documentation, payment policies, and hardship program eligibility rules. The retrieval layer used hybrid search (BM25 + embedding similarity) because Oportun's policy language had precise terminology that semantic search alone missed. Key product decision: I chunked documents at the policy rule level, not the paragraph level — because a customer asking "can I defer my payment?" needed the full eligibility condition, not a fragment of it.

---

## Q: What's the difference between fine-tuning and prompt engineering, and how do you choose?

**The answer**: Prompt engineering shapes model behavior at inference time by crafting the system prompt, examples, and instructions. Fine-tuning changes the model's weights by training it on domain-specific examples, permanently altering how it responds.

**When to use each**:
- Prompt engineering first, always — it's free, instant, reversible. Most behavior changes can be achieved through better prompting
- Fine-tuning when: the desired behavior is extremely consistent (a specific persona or output format used millions of times), the knowledge is stable (doesn't change frequently), or the prompt engineering approach consistently fails for a specific capability gap

**The PM-level trade-off**: Fine-tuning requires labeled training data (expensive to create at quality), a training pipeline (engineering investment), and model versioning infrastructure (who owns this model? how do you roll it back?). Prompt engineering requires none of these — its cost is iteration time and prompt token overhead. At scale, long prompts add up in cost; a fine-tuned model that doesn't need a 2,000-token system prompt is meaningfully cheaper per inference.

**My experience**: At Discover, I started with prompt engineering for the NLU system running on Dialogflow CX. When we hit a ceiling on intent disambiguation (financial terminology that sounded similar but had different routing logic), we added a custom entity extractor — the equivalent of fine-tuning for the specific domain vocabulary. Prompt engineering got us to 78% intent accuracy; the custom extractor got us to 91%.

---

## Q: How does an LLM know when it doesn't know something? (Hallucination and confidence)

**The answer**: It doesn't — at least not reliably. LLMs generate text that maximizes the probability of the next token given the training distribution. They don't have a separate "uncertainty" signal the way a classifier does with a confidence score. When an LLM doesn't have good information, it generates plausible-sounding text anyway — this is hallucination.

**Why this matters for PMs**: You can't ship an AI product that assumes the model only outputs what it knows. You need architectural guardrails: retrieval grounding (the model can only answer from retrieved documents), output verification (a separate check that the output is factually consistent with the retrieved context), or explicit confidence mechanisms (the model is prompted to say "I don't know" or "I'm not sure" for certain question types).

**The PM-level trade-off**: Reducing hallucination through RAG grounding reduces generalization — the model can only answer questions covered by the retrieved context. This is the right trade-off for most enterprise use cases where precision matters more than breadth. For consumer use cases where broad knowledge is valuable, you need more sophisticated hallucination mitigation (RLHF alignment, factual consistency scoring, source attribution).

**My experience**: At Oportun, hallucination was a regulatory risk, not just a quality issue. If the AI told a customer incorrect information about their payment options, it was a potential FDCPA violation. I implemented a two-stage architecture: the RAG retriever pulled the relevant policy section, the LLM generated the answer, and a separate faithfulness checker verified that the answer was entailed by the retrieved document. If not, the system routed to a human agent. This added ~400ms of latency but was non-negotiable for compliance.

---

## Q: What is RLHF and why does it matter for product decisions?

**The answer**: Reinforcement Learning from Human Feedback is the training technique that aligns LLMs to human preferences — it's what transforms a raw language model into an assistant that's helpful, harmless, and honest. The process: generate model outputs, have human raters rank them by quality, train a reward model to predict human preference, then use RL to make the base model optimize for the reward model's scores.

**Why it matters for PMs**: RLHF is why ChatGPT feels like a helpful assistant rather than a raw text predictor. But RLHF biases the model toward answers that raters like, which isn't always the same as answers that are true or useful. Raters tend to prefer confident, fluent, comprehensive responses — which can encourage confident hallucination. RLHF-aligned models are also "alignment taxed" — they sometimes refuse reasonable requests, hedge excessively, or give overly generic answers in pursuit of harmlessness.

**The PM implication**: When you're evaluating an LLM for your product, test specifically for the failure modes that RLHF alignment introduces. Is the model too verbose? Does it refuse edge cases you need it to handle? Does it give confident-sounding but wrong answers in your domain? These aren't bugs — they're the cost of the alignment training. Factor them into your model selection and prompting strategy.

---

## Q: Explain the difference between classification, generation, and retrieval in AI — and when each fits a product use case.

**Classification**: Input → one of N predefined categories. Use when: the set of valid outputs is known, decisions need to be fast and deterministic, and the model's error rate can be measured precisely (confusion matrix). Examples: intent classification in IVR, sentiment analysis, spam filtering.

**Generation**: Input → open-ended text output. Use when: the space of valid outputs is too large to enumerate, the quality of the output matters as much as its content, and some variability is acceptable. Examples: response drafting, summarization, code generation, explanation.

**Retrieval**: Query → ranked list of relevant documents. Use when: the answer exists in a known corpus, grounding and sourcing are required for trust, and freshness matters (the knowledge base updates without retraining). Examples: knowledge base search, product catalog search, document Q&A.

**Why the PM needs to know this**: These aren't just technical choices — they imply different evaluation frameworks, different failure modes, and different user expectations. Classification fails quietly (wrong category, user confused). Generation fails loudly (confidently wrong answer). Retrieval fails by returning irrelevant results (user doesn't trust the system). Each requires a different quality rubric and a different approach to human-in-the-loop design.

**My experience**: The Discover NL IVR combined all three: ASR (classification of acoustic signal → words), NLU (classification of words → intent + entity extraction), and dialog management (rule-based retrieval of the next dialog step). When we added RAG-style knowledge retrieval for FAQ-type IVR paths, we had to build a new evaluation framework for retrieval quality that didn't exist for the classification-only system.

---

## Q: How do you evaluate an AI model before shipping?

**Framework — four layers**:

1. **Offline evaluation**: Hold-out test set accuracy, precision/recall by class, error analysis on failures. Necessary but not sufficient — the test set is never the real world.

2. **Human evaluation**: Labeled quality assessment of model outputs on real or realistic inputs. Essential for generation tasks where there's no single correct answer. Expensive but irreplaceable.

3. **Shadow mode / A/B test**: Deploy the model alongside the existing system, don't surface its outputs to users yet, compare its decisions to the current system and to ground truth. Finds distribution shift issues that offline evaluation misses.

4. **Staged rollout with go/no-go criteria**: Ship to 1%, then 5%, then 25% of traffic. Define thresholds: if error rate > X, containment rate < Y, or CSAT drops by Z%, pause and investigate. Never go from 0% to 100% on an AI feature.

**My experience**: At Oportun, I built a four-stage eval framework. The go/no-go criteria for full rollout were: intent accuracy >92% on the held-out test set, containment rate within 5% of human agent rate on equivalent intents, and zero FDCPA-flagged interactions in shadow mode. The last criterion — compliance pre-check in shadow mode before any live exposure — was the design decision that saved us from a regulatory issue during the initial launch.

---

## Q: What's an embedding and why do PMs need to understand it?

**The answer**: An embedding is a numerical representation of content (text, image, audio) as a vector in high-dimensional space, where similar content is positioned near each other. The key property: semantic similarity in meaning corresponds to geometric proximity in embedding space. "Payment deferral" and "skip a payment" are close in embedding space even though they share no words.

**Why PMs need to understand it**: Embeddings are the foundation of semantic search, RAG retrieval, recommendation systems, and similarity-based clustering. When you're making product decisions about search quality, retrieval precision, or recommendation relevance, you're making decisions about embedding quality. Common PM-relevant questions: What embedding model are we using and why? How fresh are our embeddings — are they updated when content changes? What's our retrieval strategy (cosine similarity vs. BM25 hybrid)?

**The product implication**: Embedding quality determines retrieval quality, which determines answer quality in RAG systems. An embedding model trained on general text may perform poorly on domain-specific vocabulary (legal, medical, financial). This is why domain-specific embedding fine-tuning or hybrid search (combining embedding similarity with keyword matching) often outperforms pure semantic search in enterprise products.

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../../README.md)*
