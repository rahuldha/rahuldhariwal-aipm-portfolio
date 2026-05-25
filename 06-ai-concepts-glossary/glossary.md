# AI Concepts Glossary — PM Edition

*Plain-English definitions from a product management perspective. Each entry includes: what it is, the PM-relevant trade-off, and what decision it informs.*

---

## A

### Agentic AI
**What it is**: AI that doesn't just respond to a single query — it takes a sequence of actions to accomplish a multi-step task. An agentic system can plan, use tools (search, APIs, code execution), evaluate its own outputs, and iterate toward a goal without prompting for each step.

**PM trade-off**: Agents can accomplish more, but failure modes are compounded — an error in step 2 cascades through steps 3-5. Human-in-the-loop design becomes critical: at which steps does a human need to review before the agent proceeds? More autonomy = more capability + more risk.

**Decision it informs**: Whether to build an AI that assists (recommends actions for humans to take) vs. one that acts (takes actions autonomously). The trust model, UX, and compliance requirements are fundamentally different.

---

### ASR (Automatic Speech Recognition)
**What it is**: The technology that converts audio speech into text. This is the first step in any voice AI system — the text that comes out of ASR is what the NLU model works with.

**PM trade-off**: ASR quality varies dramatically by: background noise, accent, domain-specific vocabulary, and language. Error rate in ASR compounds into error rate in NLU. A 5% word error rate in ASR can cause 15-20% intent misclassification because key words (amounts, account types, intent signals) are mistranscribed.

**Decision it informs**: Vendor selection for IVR/voice products. Google's ASR (trained on 500M+ hours) outperforms most custom ASR for general speech. For domain-specific vocabulary (financial terms, medical terminology), custom language models or domain adaptation may be required.

---

## C

### Confidence Score (Calibration)
**What it is**: A model's stated certainty that its output is correct. A confidence score of 0.85 should mean the model is correct 85% of the time — when that relationship holds, the model is "well-calibrated."

**PM trade-off**: Overconfident models state high confidence on wrong answers — the most dangerous failure mode for production systems. Underconfident models escalate too much and don't add value. Calibration is a separate property from accuracy — a model can be accurate but poorly calibrated.

**Decision it informs**: Setting escalation thresholds. If the model is poorly calibrated, a confidence threshold of 0.75 may not mean what you think it means. Calibration testing on held-out data should be part of every pre-launch evaluation.

---

### Context Window
**What it is**: The maximum amount of text (measured in tokens, roughly 0.75 words each) that an LLM can consider at once. GPT-4: 128K tokens (~96K words). Claude 3.7: 200K tokens. Gemini 1.5 Pro: 1M tokens.

**PM trade-off**: Larger context window = more information the model can reason over simultaneously. But performance on long-context tasks (synthesizing a 100-page document) often degrades at the edges — models are better at information near the beginning and end of the context than in the middle ("lost in the middle" problem). Also: larger context = more expensive inference.

**Decision it informs**: Whether to use RAG (retrieve only relevant chunks) vs. full-document context injection. For most enterprise use cases with large corpora, RAG retrieval is more cost-effective and performs as well as or better than injecting entire documents.

---

## E

### Embedding
**What it is**: A numerical vector representation of text (or images, audio) that encodes semantic meaning. Similar content produces similar vectors — "payment deferral" and "skip a payment" are close together in embedding space even though they share no words.

**PM trade-off**: Embedding quality determines retrieval quality in RAG systems. General-purpose embeddings (from OpenAI, Cohere) perform well across domains. Domain-specific content (legal, medical, financial jargon) may benefit from embeddings fine-tuned on domain text. Creating and maintaining embeddings requires infrastructure: embedding pipeline, vector store, refresh cadence.

**Decision it informs**: Vendor and infrastructure selection for semantic search and RAG systems. Also: chunking strategy. How you split documents into chunks before embedding affects retrieval quality more than most engineers expect.

---

### Evaluation (Eval)
**What it is**: The systematic measurement of AI model quality — both before launch (offline evaluation on test sets) and after launch (online monitoring of production behavior).

**PM trade-off**: Offline evaluation tells you how the model performs on your test set — not on real production traffic. The gap between them is where most real failures hide. Online evaluation requires logging infrastructure and human review budgets. Under-investing in evaluation means shipping without knowing if the model works; over-investing delays launch without proportional quality improvement.

**Decision it informs**: How to allocate the engineering investment between model development and evaluation infrastructure. The right ratio is roughly 1:1 — if you spend 8 weeks building the model, plan 8 weeks for evaluation infrastructure. Teams that skip this ratio either ship poor-quality models or can't diagnose quality issues in production.

---

## F

### Fine-tuning
**What it is**: Taking a pre-trained model and training it further on a domain-specific dataset to improve performance on a specific task. The base model's weights are updated to incorporate the new examples.

**PM trade-off**: Fine-tuning improves performance on the target task but requires: (1) high-quality labeled training data (expensive to create), (2) a training pipeline (engineering investment), (3) a model versioning strategy (who maintains this model after the PM who built it moves on?). It also freezes the model's knowledge at training time — domain knowledge changes require retraining.

**Decision it informs**: Build vs. prompt engineering vs. RAG vs. fine-tuning. The decision tree: start with prompt engineering. If performance is insufficient, add RAG for knowledge-grounding. If RAG + prompt engineering is still insufficient for a specific capability, evaluate fine-tuning. Fine-tuning for knowledge (making the model know more facts) is usually worse than RAG; fine-tuning for behavior (making the model respond in a specific style or format) is usually better than prompt engineering at scale.

---

### Faithfulness
**What it is**: A quality metric for RAG and retrieval-augmented systems — does the model's response accurately reflect the retrieved source material? A response is "faithful" if every claim in it is supported by the retrieved documents.

**PM trade-off**: Faithfulness testing requires human evaluation or a separate faithfulness-scoring model. It adds latency if done at inference time. But without it, the model can confidently contradict its own source material — a hallucination that looks grounded because it was generated alongside a retrieval step.

**Decision it informs**: Whether to add a faithfulness check to the RAG pipeline before surfacing outputs to users. For regulated industries (fintech, healthcare, legal), faithfulness checking is non-negotiable. For consumer applications where variability is acceptable, it may be an optimization rather than a launch blocker.

---

## H

### Hallucination
**What it is**: When an LLM generates plausible-sounding text that is factually incorrect or unsupported by the input. The model "makes up" information with the same confidence as information it has correct.

**PM trade-off**: Hallucination is not a bug that can be fixed — it's an inherent property of how LLMs generate text (next-token prediction doesn't have a separate "is this true?" step). Mitigation strategies: RAG grounding, faithfulness checking, constrained output formats (the model can only choose from predefined options), human-in-the-loop for high-stakes outputs.

**Decision it informs**: Every AI product that surfaces LLM outputs to users needs an explicit hallucination mitigation strategy. The strategy depends on the cost of a hallucinated answer: low-stakes (user can correct) = acceptable with disclosure; high-stakes (financial, medical, legal) = requires grounding + verification architecture.

---

## L

### LLM (Large Language Model)
**What it is**: A neural network trained on large quantities of text to predict the next token in a sequence. Through this training at scale, LLMs develop emergent capabilities: reasoning, translation, code generation, summarization, and conversation.

**PM trade-off**: LLMs are general-purpose but not always the right tool. They're expensive per call, add latency, are non-deterministic (same input doesn't always produce same output), and are difficult to debug when they fail. For tasks that can be solved with rules or smaller models, LLMs add cost without proportional benefit.

**Decision it informs**: Architecture decisions at every AI product design stage. Before committing to an LLM-based approach, ask: could this be solved with a smaller task-specific model? Could it be solved with rules? If the answer to both is yes, the LLM adds risk and cost without strategic value.

---

## N

### NLU (Natural Language Understanding)
**What it is**: The component of a conversational AI system that interprets user input — classifying intent ("the user wants to change their payment date") and extracting entities ("the date they want is the 15th"). NLU sits between raw user input and the system that acts on it.

**PM trade-off**: NLU accuracy is the binding constraint on conversational AI system performance. Errors in NLU cascade — wrong intent classification leads to wrong dialog path, which leads to frustrated users and escalation. Domain-specific NLU (custom intent taxonomy + entity extractors) outperforms general-purpose models on specialized vocabulary but requires training data and maintenance.

**Decision it informs**: Whether to use a general-purpose NLU platform (Google Dialogflow CX, Amazon Lex) or build a custom NLU layer. For standard conversational use cases, platforms are faster and cheaper. For products with domain-specific vocabulary (medical, financial, legal) or unusual intent structures, custom NLU layers on top of platform foundations are usually required.

---

## P

### Prompt Engineering
**What it is**: The practice of crafting the instructions, context, and examples given to an LLM (the "prompt") to shape its output behavior. Includes: system prompts, few-shot examples, chain-of-thought instructions, output format specifications.

**PM trade-off**: Prompt engineering is fast (iterate in hours, not weeks), reversible (bad prompts can be fixed without retraining), and free (no additional training cost). But it has limits — prompting can't make a model know things it wasn't trained on, and very long prompts add inference cost at scale. Prompt changes also need to be version-controlled — a bad prompt change can degrade production quality instantly.

**Decision it informs**: Always start with prompt engineering before committing to fine-tuning or RAG. The 80/20 rule applies: 80% of model behavior improvements can be achieved through better prompting; fine-tuning and RAG cover the remaining 20% at much higher cost.

---

## R

### RAG (Retrieval-Augmented Generation)
**What it is**: An architecture where an LLM's response is grounded in documents retrieved from an external knowledge base. The flow: user query → retrieve relevant documents → inject documents into LLM context → LLM generates a grounded response.

**PM trade-off**: RAG grounds the model's responses in specific, updatable knowledge — the retrieval step is why Notion AI can answer questions about your company wiki, and why Intercom Fin can cite the policy it's referencing. The cost: retrieval adds latency, retrieval quality determines answer quality (bad retrieval = bad answers), and the knowledge base requires maintenance (someone has to update the documents).

**Decision it informs**: When to use RAG vs. fine-tuning for knowledge-intensive products. RAG is almost always better for frequently updating knowledge (policies, product docs, news). Fine-tuning is better for stable knowledge that needs to be embedded deeply in model behavior (consistent output format, tone, domain-specific style).

---

### RLHF (Reinforcement Learning from Human Feedback)
**What it is**: The training technique that aligns LLMs to human preferences. Human raters rank model outputs; a reward model learns to predict human preference; the base model is trained to maximize the reward model's scores. This is what makes ChatGPT feel like a helpful assistant rather than a raw text predictor.

**PM trade-off**: RLHF-aligned models are more helpful and less harmful than their unaligned counterparts — but alignment has a "tax." Aligned models can be overly verbose, hedge excessively, or refuse reasonable requests in pursuit of harmlessness. For enterprise products with specific, constrained use cases, the alignment defaults may need to be calibrated with system prompts or custom fine-tuning.

**Decision it informs**: When evaluating frontier models, test explicitly for RLHF-induced failure modes: over-refusal, excessive hedging, verbose responses. These aren't general model quality issues — they're alignment design decisions by the model provider that may conflict with your product requirements.

---

## T

### Token
**What it is**: The basic unit of text that LLMs process. Roughly 1 token = 0.75 words in English (punctuation, spaces, and word fragments are also tokens). "Hello world" ≈ 2 tokens. A 1,000-word document ≈ 1,333 tokens.

**PM trade-off**: LLM pricing is per token (input + output tokens). Long prompts and large context windows drive cost directly. At production scale (1M+ daily interactions), token count becomes a significant cost line item. Architecture decisions — chunking strategy, how much context to inject, output length constraints — directly affect per-interaction cost.

**Decision it informs**: Cost modeling for AI products at scale. Before committing to an architecture, estimate the average token count per interaction (input + output), multiply by per-token cost, and project against expected interaction volume. This arithmetic is non-negotiable for production AI products.

---

### Transfer Learning
**What it is**: The technique of taking a model pre-trained on a large general dataset and applying it to a new, more specific task — leveraging the knowledge encoded in the general model rather than starting from scratch.

**PM trade-off**: Transfer learning is why fine-tuning a pre-trained model (GPT, BERT, etc.) requires far less data and compute than training a model from scratch. The trade-off: the pre-trained model's biases and limitations transfer along with its capabilities. A model pre-trained on web text may have biases around domain-specific language that need correction.

**Decision it informs**: Build vs. buy for AI capabilities. "Building an LLM from scratch" is virtually never the right answer for a product team — the training cost is measured in millions of dollars and months of compute. Transfer learning (fine-tuning or prompt engineering on a pre-trained model) is the practical path for almost all enterprise AI applications.

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../README.md)*
