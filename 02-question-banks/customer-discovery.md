# Customer discovery and delivery

## Q: How would you discover the real problem?

**Ready answer:** “I would ask the user to walk me through the last time they did the task. I would observe the steps, hand-offs, workarounds, delays, and mistakes. I would talk to the people who do the work and the people who own the system, then summarize the problem back in plain language and confirm how success will be measured.”

**Follow-ups:**

- **What if users ask for a feature instead of describing a problem?** Ask what they need to accomplish and what happens today; treat the feature as a hypothesis.
- **What if stakeholders disagree?** Map each person's goal and risk, find the decision owner, and test the disputed assumption with evidence.
- **What if you cannot meet users?** Use support tickets, existing workflow records, and a proxy contact, while clearly marking assumptions and arranging user validation as soon as possible.

## Q: How do you know the customer problem is solved?

Set a baseline and an outcome that matters to the user: completion time, error rate, successful task completion, adoption, or another agreed measure. Pair quantitative signals with user feedback. Define guardrails such as unsafe recommendations, privacy incidents, or an unacceptable increase in manual review. Agree on a review date.

## Q: A user says, “This AI is wrong.” What do you do?

Thank them, capture the exact input and expected outcome with appropriate privacy controls, and reproduce the issue. Identify whether the cause is data, prompt, retrieval, model, integration, or a mismatch in expectations. Explain what you know without blaming the user. Fix or mitigate, then show the user what changed and how you will catch a repeat.

## Q: How would you roll out a new AI workflow?

Start with a small group and low-risk tasks. Make the system's limits and escalation path clear. Train users on the workflow, not only the interface. Monitor quality, latency, cost, fallback use, and user adoption. Expand only when the agreed acceptance criteria hold; keep a rollback plan.

## Q: How do you balance a customer request with a reusable product?

Clarify whether the need is unique or likely shared. Prefer configuration or a narrow extension when it preserves a coherent product. If custom code is needed, name its owner, maintenance cost, upgrade impact, and conditions for generalizing it. Confirm the trade-off with the customer and product team.

## Case practice: multilingual field support

A field team in India needs quick guidance from photos and short voice notes. Connectivity is inconsistent; users have mixed literacy; unsafe advice can cause real loss.

**Your first response:** ask which decision the guidance supports, who is accountable, what source material is authoritative, what happens when the model is uncertain, what devices and languages are used, and how success and harm are measured. Then propose a limited pilot with human review for high-impact advice, clear source grounding, offline or retry behavior, and an explicit fallback to a person.

**Interviewer probes, with answers:**

- **What do you ship in week one?** One decision for one user group. Answers come from trusted source tables or rules, with a clear "ask a person" button. No free-form AI advice yet.
- **What if users share poor photos?** Check the photo quality first and ask for a retake with a simple tip, rather than giving a confident wrong answer. Log how often retakes happen.
- **How do you test code-switching (mixing languages, such as Hinglish)?** Build a test set of real mixed-language messages and run it in CI with a minimum pass rate. In Poultry 360, this kind of set found that only 5 of 13 Hinglish trouble reports were routed correctly before the fix.
- **How do you keep advice current?** Advice comes from versioned source material with an owner and a review date. When the source changes, re-run the evaluations before release.
- **What data should not be retained?** Photos and voice notes beyond processing, unless the user agrees, plus phone numbers in logs and anything not needed for the task.
- **What would make you stop the pilot?** Any unsafe advice that reached a user, field users not adopting it, or human review becoming a bottleneck. Agree on these stop rules before starting.
