# FDE fundamentals

## What makes an FDE effective?

An effective FDE connects customer context to working software. They can ask useful questions, make trade-offs visible, build across system boundaries, communicate clearly, and learn from production behavior. The job is not “use every AI tool”; it is “make the customer's workflow better and keep the system dependable.”

## Q: A customer asks for an AI agent. What do you do first?

**Ready answer:** “I would ask what job they want done, who does it today, and what makes the current process painful. I would map inputs, decisions, systems, permissions, and failure consequences. Then I would agree on a measurable outcome and test whether a simpler workflow or deterministic automation solves it before proposing an agent.”

**Probe: What questions would you ask?**

- Which user and task are we improving?
- How often does it happen and how long does it take today?
- What are the costly or risky mistakes?
- Which data and systems are involved, and who can access them?
- What must remain under human control?
- How will users know the result is correct?
- What is the deadline, budget, deployment environment, and support model?

**Probe: What if the customer insists on an agent?**

Acknowledge the request, then make the smallest safe experiment. Compare agent behavior with a simpler baseline. Agree on acceptance criteria, permissions, escalation paths, and a stop condition before expanding.

## Q: How do you scope an FDE engagement?

**Ready answer:** “I would define one user, one workflow, one measurable outcome, and one thin end-to-end slice. I would include integration and a way to observe failures in the first scope. I would defer features that do not test the core assumption.”

**Follow-ups:**

- **How do you handle a fixed deadline?** Rank work by user value and risk; deliver a safe thin slice; state what is not covered.
- **How do you deal with changing requirements?** Revisit the success measure and trade-offs with the customer; keep a written decision log.
- **How do you avoid over-customization?** Separate reusable product needs from customer-specific configuration and document the maintenance cost.

## Q: Tell me about a difficult trade-off.

Use one real product decision. Describe the options, user impact, risks, why you chose one, and what evidence would make you revisit it. Possible story anchors: CPU-friendly ONNX inference in Poultry 360; enforcing user dislikes in code in Aaina; human approval in Aura with Rav. State actual constraints; do not invent a cost or performance comparison.

## Q: What if the solution does not work in the customer's environment?

**Ready answer:** “First I would reproduce the failure with the customer's permissions, data shape, network, and deployment setup. I would separate product defects from integration or configuration issues, agree on an immediate workaround if safe, and communicate the impact and next update clearly. Then I would fix the root cause, add a regression check, and verify with the customer’s workflow.”

## Q: How do you work with non-technical users?

Use their words for the workflow, demonstrate with a realistic task, and check understanding by asking them to complete the task. For multilingual or low-literacy users, offer the interaction style that works for them, such as local language and read-aloud, and test comprehension rather than assuming a translated screen is enough.
