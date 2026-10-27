# 🤖 AI systems and production judgment

---

## 💬 Q1. When would you use an LLM, RAG, an agent, or ordinary code?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "I start from the task and the cost of a wrong result. If rules are stable, ordinary code is easier to test. If the system must answer from a changing knowledge base, retrieval may help. If a task needs flexible multi-step choices or tool use, an agent may help, but it adds uncertainty and control requirements. I compare the simplest viable baseline with the AI approach on real examples."

| Situation | Reach for |
|---|---|
| 📏 Stable rules | Ordinary code |
| 📚 Answers from a changing knowledge base | RAG |
| 🧭 Flexible multi-step choices or tool use | An agent, with tight controls |
| 🤷 Not sure yet | Test the simplest baseline first |

🔗 Deeper version: [Session 06, Q8 "AI for a job SQL can do"](../01-bootcamp-sessions/2026-10-25-exceptions-files-iso/README.md)

---

## 💬 Q2. How would you evaluate an AI feature?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I build a representative set of realistic inputs with expected outcomes: normal, edge, ambiguous, other-language and adversarial cases, plus known past failures. I track task success and error categories, not just one score, and use human review where judgment matters. I run it before and after every change, then monitor production feedback with privacy controls."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>How do you avoid overfitting prompts to the evaluation set?</b></summary>

Keep a **held-out set**, keep adding new real failure cases, and review whether the tests still reflect real usage.

</details>

<details>
<summary><b>What if you have no labels?</b></summary>

Start with expert review, pairwise comparisons or outcome proxies. Make the uncertainty visible and build a labelling loop.

</details>

<details>
<summary><b>What belongs in CI?</b></summary>

**Fast, stable checks** for critical behaviour and known regressions. Keep expensive or subjective evaluations in a scheduled or release process.

</details>

🔗 Deeper version: [Session 05, Q8 "Do you switch to the newer model?"](../01-bootcamp-sessions/2026-10-24-network-io-memory-streaming/README.md)

---

## 💬 Q3. Your RAG answer is wrong. How do you debug it?

> [!TIP]
> **🗣️ Say this:** "I trace the path stage by stage, look at the exact passages that were retrieved and their permissions, and classify the failure before I change the model."

```
📄 source freshness & access → ✂️ parsing / chunking → 🔎 retrieval → 🏅 ranking / context
   → 🤖 model response → ✅ output validation → 🖥️ user display
```

Then test a targeted fix against a held-out set, measuring answer quality, latency, cost and citation behaviour.

---

## 💬 Q4. How do you make an agent safer?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I limit tools and permissions to the task, validate tool inputs and outputs, and keep untrusted content separate from instructions. I set step and time limits, make important actions reversible or approval-gated, log structured events without collecting unnecessary sensitive data, and test refusal, injection, tool errors and escalation."

| 🛡️ Control | Why |
|---|---|
| Least-privilege tools | Limits the blast radius |
| Validate tool I/O | Catches bad calls early |
| Separate untrusted content | Stops prompt injection |
| Step and time limits | Prevents runaway loops |
| Approval for risky actions | A human owns irreversible steps |
| Structured logs | Lets you investigate later |

> [!IMPORTANT]
> Use Aura with Rav as the example only as far as you can explain which checks exist, what they **don't** guarantee, and how human approval works → [story bank](../03-story-bank/README.md)

---

## 💬 Q5. How do you decide whether an agent needs human review?

> [!TIP]
> **🗣️ Say this (≈40 s)**
>
> "I look at how severe and reversible a mistake is, how uncertain the task is, and whether the user can spot the error. High-impact or irreversible actions get stronger review or stay manual. Triggers come from evidence, not from an uncalibrated model confidence score alone."

---

## 💬 Q6. The AI feature is slow or costly. What do you try?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "I measure first: request volume, model calls, context size, retries, tools and downstream services. Then I try caching where freshness allows, smaller models for suitable tasks, shorter context, batching or async work, and deterministic shortcuts. I re-check quality and safety after every optimisation."

---

## 💬 Q7. What does "production-ready" mean to you?

> [!TIP]
> **🗣️ Say this:** "A successful demo is not enough. Production-ready means every box below is ticked."

- [x] A clear owner and user
- [x] Works in its real deployment environment
- [x] Handles expected failures
- [x] Protects data and permissions
- [x] Observable (logs, metrics, alerts)
- [x] Has a recovery or fallback path
- [x] Evidence it meets the agreed acceptance criteria

---

🏠 [Back to README](../README.md)
