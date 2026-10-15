# 🤝 Customer discovery and delivery

---

## 💬 Q1. How would you discover the real problem?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "I would ask the user to walk me through the last time they did the task. I would watch the steps, hand-offs, workarounds, delays and mistakes. I would talk to the people who do the work and the people who own the system, then summarise the problem back in plain language and confirm how success will be measured."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>What if users ask for a feature instead of describing a problem?</b></summary>

Ask what they need to accomplish and what happens today. Treat the feature as a **hypothesis**, not a requirement.

</details>

<details>
<summary><b>What if stakeholders disagree?</b></summary>

Map each person's goal and risk, find the **decision owner**, and test the disputed assumption with evidence.

</details>

<details>
<summary><b>What if you cannot meet users?</b></summary>

Use support tickets, existing workflow records and a proxy contact. Clearly mark your assumptions, and arrange validation with real users as soon as possible.

</details>

---

## 💬 Q2. How do you know the customer problem is solved?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "I set a baseline and an outcome that matters to the user, such as completion time, error rate, successful tasks or adoption, and pair the numbers with user feedback. I define guardrails, such as no unsafe recommendations, no privacy incidents and no big rise in manual review, and we agree on a review date."

---

## 💬 Q3. A user says, "This AI is wrong." What do you do?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I thank them, capture the exact input and the expected result with privacy controls, and reproduce it. I find whether the cause is data, prompt, retrieval, model, integration or a mismatch in expectations. I explain what I know without blaming the user, fix or mitigate it, then show them what changed and how we'll catch a repeat."

| Step | |
|:---:|---|
| 1️⃣ | 🙏 Thank and capture the exact case |
| 2️⃣ | 🔁 Reproduce it |
| 3️⃣ | 🔍 Classify the cause: data · prompt · retrieval · model · integration · expectation |
| 4️⃣ | 🛠️ Fix or mitigate |
| 5️⃣ | 📣 Show the user what changed, and add a regression test |

---

## 💬 Q4. How would you roll out a new AI workflow?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "Start with a small group and low-risk tasks. Make the system's limits and escalation path clear, and train users on the workflow, not just the screen. Monitor quality, latency, cost, fallback use and adoption. Expand only when the agreed acceptance criteria hold, and keep a rollback plan."

---

## 💬 Q5. How do you balance a customer request with a reusable product?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "First I check whether the need is unique or likely shared. I prefer configuration or a narrow extension that keeps the product coherent. If custom code is needed, I name its owner, maintenance cost, upgrade impact and the conditions for generalising it, and confirm the trade-off with the customer and the product team."

---

## 🧪 Case practice: multilingual field support

> [!NOTE]
> **📋 Scenario:** a field team in India needs quick guidance from photos and short voice notes. Connectivity is patchy, literacy is mixed, and unsafe advice can cause real loss.

> [!TIP]
> **🗣️ Your first response**
>
> Ask:
> - which decision the guidance supports
> - who is accountable
> - what source material is authoritative
> - what happens when the model is unsure
> - what devices and languages are used
> - how success and harm are measured
>
> Then propose a limited pilot with:
> - human review for high-impact advice
> - answers grounded in clear sources
> - offline or retry behaviour
> - an explicit fallback to a person

**❓ Interviewer probes** · *click to open*

<details>
<summary><b>What do you ship in week one?</b></summary>

One decision for one user group. Answers come from **trusted source tables or rules**, with a clear "ask a person" button. No free-form AI advice yet.

</details>

<details>
<summary><b>What if users share poor photos?</b></summary>

Check photo quality first and **ask for a retake** with a simple tip, rather than giving a confident wrong answer. Log how often retakes happen.

</details>

<details>
<summary><b>How do you test code-switching (mixing languages, such as Hinglish)?</b></summary>

Build a test set of **real mixed-language messages** and run it in CI with a minimum pass rate. In Poultry 360, such a set found that only **5 of 13** Hinglish trouble reports were routed correctly before the fix.

</details>

<details>
<summary><b>How do you keep advice current?</b></summary>

Advice comes from **versioned source material** with an owner and a review date. When a source changes, re-run the evaluations before release.

</details>

<details>
<summary><b>What data should not be retained?</b></summary>

- Photos and voice notes beyond processing (unless the user agrees)
- Phone numbers in logs
- Anything not needed for the task

</details>

<details>
<summary><b>What would make you stop the pilot?</b></summary>

- ❌ Any unsafe advice reaching a user
- ❌ Field users not adopting it
- ❌ Human review becoming a bottleneck

**Agree on these stop rules before starting.**

</details>

---

🏠 [Back to README](../README.md)
