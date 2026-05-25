# AI PM Toolkit

*These aren't theoretical frameworks — they're the actual decision-making tools applied to production AI systems at Oportun and Discover Financial Services.*

---

## Frameworks

| Framework | File | What It Solves |
|-----------|------|---------------|
| AI Feature Prioritization | [ai-feature-prioritization.md](./ai-feature-prioritization.md) | Modified RICE scoring that adds Data Readiness and Failure Mode Risk as AI-specific modifiers |
| AI Rollout Framework | [ai-rollout-framework.md](./ai-rollout-framework.md) | Phased shadow → controlled → expanded → full rollout with go/no-go gates at each stage |
| AI Evaluation Rubric | [ai-evaluation-rubric.md](./ai-evaluation-rubric.md) | Three-layer eval (model metrics, product metrics, business metrics) with infrastructure checklist |

---

## When to Use Each

**Starting a new AI feature**: AI Feature Prioritization first — score it before committing engineering resources. If Data Readiness is low, the first milestone is data collection, not model development.

**Preparing to launch**: AI Rollout Framework — define your go/no-go criteria and rollback thresholds before writing a line of code. The criteria should be signed off by PM, engineering, and compliance before shadow mode begins.

**Measuring ongoing performance**: AI Evaluation Rubric — use the three-layer structure to build your monitoring dashboard. If you're only measuring one layer, you're missing the full picture.

---

*Part of [Rahul Dhariwal's AI PM Portfolio](../README.md)*
