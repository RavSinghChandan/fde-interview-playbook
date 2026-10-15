# AI systems and production judgment

## Q: When would you use an LLM, RAG, an agent, or ordinary code?

**Ready answer:** “I start from the task and the cost of a wrong result. If rules are stable, ordinary code is easier to test. If the system must answer from a changing knowledge base, retrieval may help. If a task needs flexible multi-step choices or tool use, an agent may help, but it adds uncertainty and control requirements. I would compare the simplest viable baseline with the proposed AI approach on real examples.”

## Q: How would you evaluate an AI feature?

Build a representative set of realistic inputs with expected outcomes or review guidance. Include normal cases, edge cases, ambiguous inputs, language variation, adversarial inputs, and known past failures. Track task-level success and error categories, not only one aggregate score. Use human review where judgment matters. Run evaluations before and after changes, then monitor production feedback with privacy controls.

**Follow-ups:**

- How do you avoid overfitting prompts to the evaluation set? Keep a held-out set, add new failure cases, and review whether tests reflect real usage.
- What if you do not have labels? Start with expert review, pairwise comparisons, or outcome proxies; make uncertainty visible and build a labeling loop.
- What belongs in CI? Fast, stable checks for critical behavior and known regressions. Keep expensive or subjective evaluations in a scheduled or release process where appropriate.

## Q: Your RAG answer is wrong. How do you debug it?

Trace the path: source freshness and access → parsing/chunking → retrieval → ranking/context → model response → output validation and user display. Inspect the exact retrieved passages and permissions. Classify the failure before changing the model. Test a targeted fix against a held-out set and measure answer quality, latency, cost, and citation behavior.

## Q: How do you make an agent safer?

Limit tools and permissions to the task. Validate tool inputs and outputs. Separate untrusted content from instructions. Set step and time limits. Make important actions reversible or require approval. Record enough structured events to investigate behavior without collecting unnecessary sensitive data. Add tests for refusal, injection, tool errors, and escalation.

Use Aura with Rav as your example only to the extent you can explain what checks exist, what they do not guarantee, and how human approval works.

## Q: How do you decide whether an agent needs human review?

Consider severity and reversibility of mistakes, uncertainty, and user ability to detect errors. High-impact or irreversible actions should have stronger review or remain manual. Define confidence or policy triggers from evidence; do not rely on an uncalibrated model confidence score alone.

## Q: The AI feature is slow or costly. What do you try?

Measure where time and cost go first: request volume, model calls, context size, retries, tools, and downstream services. Consider caching where data and freshness allow it, smaller models for suitable tasks, shorter context, batching or async work, and deterministic shortcuts. Re-check quality and safety after every optimization.

## Q: What does “production-ready” mean to you?

It has a clear owner and user, works in its deployment environment, handles expected failures, protects data and permissions, is observable, has a recovery or fallback path, and has evidence that it meets agreed acceptance criteria. A successful demo alone is not enough.
