# 🧩 FDE fundamentals

> [!NOTE]
> **What makes an FDE effective?** They connect customer context to working software: ask useful questions, make trade-offs visible, build across system boundaries, communicate clearly and learn from production. The job is not "use every AI tool". It is **"make the customer's workflow better and keep the system dependable."**

---

## 💬 Q1. A customer asks for an AI agent. What do you do first?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "I would ask what job they want done, who does it today, and what makes the current process painful. I would map inputs, decisions, systems, permissions and failure consequences. Then I would agree on a measurable outcome and test whether a simpler workflow or deterministic automation solves it before proposing an agent."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>What questions would you ask the customer?</b></summary>

| Area | Question |
|---|---|
| 👤 User | Which user and task are we improving? |
| ⏱️ Today | How often does it happen, and how long does it take now? |
| 💥 Risk | What are the costly or risky mistakes? |
| 🔐 Data | Which data and systems are involved, and who can access them? |
| 🧑‍⚖️ Control | What must remain under human control? |
| ✅ Trust | How will users know the result is correct? |
| 📅 Limits | Deadline, budget, deployment environment, support model? |

</details>

<details>
<summary><b>What if the customer insists on an agent?</b></summary>

Acknowledge the request, then run the **smallest safe experiment**. Compare the agent with a simpler baseline. Agree on acceptance criteria, permissions, escalation paths and a **stop condition** before expanding.

</details>

---

## 💬 Q2. How do you scope an FDE engagement?

> [!TIP]
> **🗣️ Say this (≈40 s)**
>
> "One user, one workflow, one measurable outcome, and one thin end-to-end slice. I include integration and a way to observe failures in the first scope, and defer features that don't test the core assumption."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>How do you handle a fixed deadline?</b></summary>

Rank work by **user value and risk**, deliver a safe thin slice, and state clearly what is not covered.

</details>

<details>
<summary><b>How do you deal with changing requirements?</b></summary>

Revisit the success measure and trade-offs with the customer, and keep a **written decision log**.

</details>

<details>
<summary><b>How do you avoid over-customization?</b></summary>

Separate reusable product needs from customer-specific configuration, and document the **maintenance cost** of each custom piece.

</details>

---

## 💬 Q3. Tell me about a difficult trade-off.

> [!TIP]
> **🗣️ Shape of the answer:** options → user impact → risks → why you chose one → what evidence would make you revisit it.

| Story anchor | Trade-off |
|---|---|
| 🐔 Poultry 360 | CPU-friendly ONNX inference vs a bigger model needing a GPU |
| 💇 Aaina | Rules enforced in code vs prompt-only rules |
| 🔮 Aura with Rav | Human approval (slower) vs fully automatic reports |

> [!WARNING]
> State actual constraints. Do not invent a cost or performance comparison.

---

## 💬 Q4. What if the solution doesn't work in the customer's environment?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "First I would reproduce the failure with the customer's permissions, data shape, network and deployment setup. I would separate product defects from integration or configuration issues, agree on an immediate workaround if it's safe, and communicate the impact and the time of the next update clearly. Then I fix the root cause, add a regression check, and verify it with the customer's workflow."

🔗 Deeper version: [Session 01, Q2 "works on my laptop"](../01-bootcamp-sessions/2026-10-10-dev-environment-uv/README.md)

---

## 💬 Q5. How do you work with non-technical users?

> [!TIP]
> **🗣️ Say this (≈40 s)**
>
> "I use their words for the workflow, demonstrate with a realistic task, and check understanding by asking them to do the task. For multilingual or low-literacy users I offer what works for them, such as their local language and read-aloud, and I test whether they understood rather than assuming a translated screen is enough."

---

🏠 [Back to README](../README.md)
