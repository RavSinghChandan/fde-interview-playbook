# 🧠 Behavioral and ownership questions

> [!CAUTION]
> Every story here must be **true**. Pick real examples; never present a disguised success as a failure, or another person as unreasonable.

---

## 💬 Q1. Tell me about a time you found a problem others hadn't noticed.

> [!TIP]
> **🗣️ Story anchor: Aaina.** The evaluation found the model dropped customers' dislikes in **3 of 15** answers. Explain:
> - how you noticed the risk
> - how you made the behaviour testable in code
> - why you chose a repair retry
> - what "100%" means for that tested set

> [!WARNING]
> Don't claim universal performance from a 15-case evaluation.

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why not just improve the prompt?</b></summary>

> "A prompt is a request, not a guarantee. With rules only in the prompt, 3 of 15 live profiles still broke them. A customer's dislike must never be ignored, so I moved the rule into code: the catalog is filtered before the model sees it, and the answer is checked afterwards. A model can't pick an option it was never given. To broaden the test set, I'd add held-out profiles I didn't tune on, more languages, and every real complaint as a new case."

</details>

---

## 💬 Q2. Tell me about a time you handled a production constraint.

> [!TIP]
> **🗣️ Story anchor: Poultry 360**, which runs bird counting with an ONNX model on a plain CPU. Explain the device and latency constraints you targeted, how you chose the model and runtime, and the trade-offs you accepted.

> [!IMPORTANT]
> **✍️ Rav:** have measured latency and accuracy ready if you have them. If not, say what you have and haven't measured.

---

## 💬 Q3. Tell me about a mistake or failure.

> [!TIP]
> **🗣️ Shape:** a real incident → honest impact → your role → how you communicated → the correction → the safeguard you added.

It should show **accountability and learning**, not a disguised success story.

---

## 💬 Q4. How do you handle feedback on unfamiliar code?

> [!TIP]
> **🗣️ Story anchor: open source.** You work in established libraries: read the project's conventions, make a focused change, respond to review and revise until maintainers merge it.

"121 merged PRs" is the headline. The interviewer wants **one concrete PR**: the issue, tests or evidence, review feedback, and what you changed → [story 6](../03-story-bank/README.md)

---

## 💬 Q5. Describe a disagreement with a stakeholder.

> [!TIP]
> **🗣️ Shape:** each person's goal → the evidence you gathered → how you made the trade-offs visible → how the decision was reached.

---

## 💬 Q6. How do you prioritise when everything is urgent?

> [!TIP]
> **🗣️ Say this (≈40 s)**
>
> "I clarify customer impact, deadline, safety or security risk, dependencies and reversibility. Then I share a short ranked plan that says what will slip, and reassess when new evidence arrives. I don't promise several top priorities at once."

---

## 💬 Q7. Why are you moving toward FDE?

> [!TIP]
> **🗣️ Say this (≈40 s)**
>
> "I have repeatedly enjoyed work that starts with a practical problem and ends with software people can use. My background combines enterprise integrations and distributed systems with end-to-end AI products. FDE is a natural next step because it puts customer discovery, engineering and adoption in the same responsibility."

---

## 💬 Q8. What is an area you're improving?

> [!TIP]
> **🗣️ Shape:** a genuine skill gap relevant to the role → what you practise now → evidence of progress.

*Possible example (use only if true):* making customer discovery more structured: watch the workflow, write down assumptions, agree on a measurable outcome, and validate the result with users.

---

🏠 [Back to README](../README.md)
