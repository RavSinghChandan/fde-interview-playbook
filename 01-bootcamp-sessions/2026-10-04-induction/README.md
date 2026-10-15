# 🏁 Session 00 — Bootcamp induction

![Date](https://img.shields.io/badge/date-4%20Oct%202026-2563eb?style=flat-square)
![Type](https://img.shields.io/badge/type-induction%20%2B%20Q%26A-9333ea?style=flat-square)
![Questions](https://img.shields.io/badge/interview%20questions-5-16a34a?style=flat-square)

> [!NOTE]
> **📼 Source:** supplied VTT transcripts `GMT20261004-021909_Recording.transcript.vtt` (main session) and `GMT20261004-084659_Recording.transcript.vtt` (short afternoon Q&A). Neither file is stored in this repository.
>
> **🧪 Technical lesson:** none. This was an induction. The interview questions are preparation built from the session themes and your CV, not questions the instructor asked.

---

## 📚 Useful ideas from the session

| | Idea |
|:---:|---|
| 📅 | The bootcamp is planned for about **five to six months**. Check your dashboard for current access details |
| 🎯 | The goal is not to learn a fixed list of tools. **Apply each concept to your own domain and project** |
| 🧭 | **FDE** is framed around the customer and end-to-end delivery. **MLOps** is more about operating and optimising ML systems. Real teams overlap |
| 🔬 | Go **deep in core topics** and be ready for follow-up questions, rather than touching every technology lightly |
| ✅ | **Validation is essential.** A running demo is not enough. Know how you would detect hallucination, failure or a bad user outcome |
| 🗂️ | Cohort questions are not the syllabus. Each later class gets its own folder |

## ⏱️ 10-minute recap

An FDE does not start by picking a fashionable framework. They:

1. 🔍 find the customer's **real workflow, constraints and success measure**
2. 🛠️ build a **practical solution**
3. 🔌 **integrate and validate** it
4. 🤝 help it **work in the customer's environment**

Your best proof of this approach is **Poultry 360** and your **enterprise integration work**.

---

## 💬 Q1. What does Forward Deployed Engineering mean to you?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "It means taking engineering close to the customer's real problem. I work with the user to understand the workflow and constraints, build and integrate a useful solution, validate it with real cases, and stay accountable through deployment and feedback."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>How is that different from a product engineer?</b></summary>

The FDE works more directly in a customer's context and often handles discovery and integration. A product engineer may build for a broader, shared roadmap. In practice the roles overlap.

</details>

<details>
<summary><b>How is it different from consulting?</b></summary>

Consulting may end with recommendations. An FDE is expected to **build, integrate and prove the solution works**, although companies define the role differently.

</details>

<details>
<summary><b>How is it different from MLOps?</b></summary>

MLOps focuses on the lifecycle and operation of ML systems. FDE includes customer discovery and end-to-end workflow delivery, and may include MLOps when needed.

</details>

<details>
<summary><b>How would you measure success?</b></summary>

**User adoption and task outcome first**, then reliability, latency, cost and support burden as appropriate.

</details>

---

## 💬 Q2. Tell me about a time you turned an unclear need into a working product.

> [!TIP]
> **🗣️ Use Poultry 360**
>
> Cover:
> - the farmer's task
> - why hand counting and generic advice were not enough
> - the low-literacy and language constraints
> - your product choices
> - how you evaluated the behaviour

> [!WARNING]
> Do not imply user-research steps you did not perform.

**❓ Follow-ups** · *click to open*

<details>
<summary><b>How did you select the first feature?</b></summary>

The highest-cost daily guess: **how much feed to give by bird age**. Feed is about 60–70% of the cost of raising a broiler, and the answer can come from published tables.

</details>

<details>
<summary><b>What was the least expensive useful version?</b></summary>

**Table lookups by age with no AI at all**: exact, instant and free. The model was added later only to *explain* a number, never to produce one.

</details>

<details>
<summary><b>What if farmers had unreliable internet?</b></summary>

The UI loads from a CDN in about a second, and every answer has a table or rule path that works without the AI call.

⚠️ A fully offline mode is **not built yet**. Say so if asked.

</details>

<details>
<summary><b>How did you validate the advice?</b></summary>

- Numbers come only from the **NRC 1994 / BIS IS 1374** tables.
- A **grounding guard** flags any figure in the model's reply that is not in the table.
- Routing is tested by **40 English / Hindi / Hinglish questions in CI**.

</details>

<details>
<summary><b>What would make you remove the AI component?</b></summary>

If explanations confused farmers, or the guard kept catching invented figures. The product already works without the model, so removing it costs only the explanations.

</details>

---

## 💬 Q3. How do you stop yourself from building a demo instead of a product?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I define a real user task and success condition, test realistic and failure cases, integrate with the workflow, and observe what happens after launch. For my products, evaluations caught concrete failures, such as Hinglish trouble reports being routed incorrectly and customer dislikes being dropped. I turned those findings into checks and fixes."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>What cases were in the evaluation?</b></summary>

| Product | Evaluation set |
|---|---|
| 🐔 Poultry 360 | 40 routing questions in English, Hindi and Hinglish |
| 💇 Aaina | 15 live customer profiles checked against the catalog and dislike rules |

</details>

<details>
<summary><b>How do you prevent test data leakage?</b></summary>

Be honest: I wrote these sets and made the fixes against them, so the scores are **in-sample**. The cure is a **held-out set I do not tune on**, plus new real failures added as they appear.

</details>

<details>
<summary><b>Good offline score vs good customer outcome?</b></summary>

An offline score shows that *known* cases pass. A customer outcome is whether the farmer fed correctly or the salon customer came back. Real users bring inputs the test set never had.

</details>

<details>
<summary><b>What do you monitor after deployment?</b></summary>

- 📈 **Per route:** error rate and p95 latency
- 🤖 **Per AI step:** how often the model's answer was kept, repaired or replaced, and the AI fallback rate
- 🚦 **Daily-cap hits** (HTTP 429s)
- 🔎 All tied together by a **request ID** in one JSON log line

</details>

---

## 💬 Q4. How do you choose what to learn or build from a course?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "I turn each concept into a decision I might face at work: what problem does it solve, when is it a poor fit, how will I test it, and what is the smallest implementation I can explain? Then I connect it to one of my existing products rather than collecting notes on tools."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Show one example.</b></summary>

The class on virtual environments became a real check: my products pin dependencies and build from a Docker image, so the server runs exactly what I tested.

</details>

<details>
<summary><b>How will you avoid shallow coverage?</b></summary>

For each topic I must be able to answer **"when is it a poor fit?"** and **"how would I test it?"** If I can't, I go deeper before moving on.

</details>

<details>
<summary><b>How do you balance preparation with active job applications?</b></summary>

Short daily drills (20 minutes) and one story at a time, so preparation never blocks applying.

</details>

---

## 🌇 Same-day follow-up Q&A (afternoon recording)

A short career Q&A with no technical lesson. The instructors' points, without participant details:

| | Point |
|:---:|---|
| 📸 | **AI is usually an enhancement on top of existing software, not a replacement.** A phone camera uses AI to improve photos, and a chat app is not itself AI. Enterprise stacks such as Java are not going away |
| 🧑‍💻 | **"AI FDE" vs FDE:** engineers who already own client-facing delivery may already be doing FDE work. The AI part is what the course adds |
| 🏗️ | For experienced engineers, the **FDE route was described as sufficient**. Architect roles focus more on design and guiding developers, so read each job description |
| 🏦 | **In banks and insurers it depends on the project.** Most companies are trying to add AI, so look for openings where you can apply it |

## 💬 Q5. You come from Java and enterprise systems. Why FDE, and how does that help?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "Most AI value in a company sits on top of systems that already exist: banking services, airline transactions, internal tools. I have built those systems in Java, so I understand integration, reliability and the constraints of regulated environments. I have also built and run AI products end to end. An FDE needs both: someone who can connect a model to a real workflow safely, inside the customer's existing stack."

> [!CAUTION]
> **Honesty rule:** describe your past work as it was. Show how it prepares you for AI integration, but **never relabel non-AI work as AI work**.

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Which AI feature would you add to a system you built before, and how would you measure it?</b></summary>

*Pattern answer:* an assistant that summarises a failed transaction's logs for support staff. Measure **time to resolve a ticket** and **how often staff accept the summary**, against a baseline.

Pick a system you actually worked on and keep confidential details out.

</details>

<details>
<summary><b>What would you refuse to automate?</b></summary>

**Irreversible, high-impact decisions**, such as moving money or turning a customer down without a human check. AI can suggest and explain; a person approves.

</details>

<details>
<summary><b>How do you handle data that cannot leave the customer's network?</b></summary>

Use a model hosted inside their cloud or network, or a self-hosted open model. **Mask personal data** before any call, and confirm the approach with their security team first.

</details>

---

## 🏋️ This week's practice (20 minutes)

- [ ] Give the FDE definition in **45 seconds**
- [ ] Explain one product using [CLEAR](../../00-start-here/answer-method.md)
- [ ] After every claim, ask yourself: *why? what failed? how do you know? what would you change?*
- [ ] Find any metric you cannot defend and correct it before using the story

## 🔍 Open items for later sessions

- Add the official module or class topic once technical sessions start
- Replace assumptions about customer discovery with your exact evidence
- Record the bootcamp's actual project and evaluation requirements once taught

---

⬅️ [Back to README](../../README.md) · ➡️ [Session 01 — Developer environment](../2026-10-10-dev-environment-uv/README.md)
