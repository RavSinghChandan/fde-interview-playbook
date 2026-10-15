# Behavioral and ownership questions

## Q: Tell me about a time you found a problem others had not noticed.

**Story anchor:** Aaina evaluation found the model dropped customers' dislikes in 3 of 15 answers. Explain how you noticed the risk, how you made the behavior testable in code, why a repair retry was chosen, and what “100%” means for that tested set. Do not claim universal performance from a 15-case evaluation.

**Probe: Why not just improve the prompt?**

**Answer:** "A prompt is a request, not a guarantee. With rules only in the prompt, 3 of 15 live profiles still broke them. A customer's dislike must never be ignored, so I moved the rule into code: the catalog is filtered before the model sees it, and the answer is checked afterwards. A model cannot pick an option it was never given. To broaden the test set, I would add held-out profiles I did not tune on, more languages, and every real complaint as a new case."

## Q: Tell me about a time you handled a production constraint.

**Story anchor:** Poultry 360 runs bird counting with an ONNX model on a plain CPU. Explain the actual device and latency constraints you targeted, how you selected the model/runtime, and what trade-offs you accepted. Be ready with measured latency and accuracy if available; otherwise say what you have and have not measured.

## Q: Tell me about a mistake or failure.

Choose a real incident from your own work. State the impact honestly, your role, how you communicated, the correction, and what safeguard you added. The example should show accountability and learning, not a disguised success story.

## Q: How do you handle feedback on unfamiliar code?

Use open-source work: you work in established libraries, read project conventions, make a focused change, respond to review, and revise until maintainers merge it. Explain one specific PR with the issue, tests or evidence, review feedback, and what you changed. “106 merged PRs” is a headline; the interviewer wants one concrete example.

## Q: Describe a disagreement with a stakeholder.

Pick a real example. Explain each person's goal, the evidence you gathered, how you made trade-offs visible, and how the decision was reached. Avoid presenting the other person as unreasonable.

## Q: How do you prioritize when everything is urgent?

Clarify customer impact, deadline, safety/security risk, dependencies, and reversibility. Communicate a short ranked plan and what will slip. Reassess when new evidence arrives. Do not promise multiple top priorities at once.

## Q: Why are you looking to move toward FDE?

“I have repeatedly enjoyed work that starts with a practical problem and ends with software people can use. My background combines enterprise integrations and distributed systems with end-to-end AI products. FDE is a natural next step because it puts customer discovery, engineering, and adoption in the same responsibility.”

## Q: What is an area you are improving?

Choose a genuine skill gap relevant to the role, then name your current practice and evidence of progress. A useful example could be making customer discovery more structured: observe the workflow, write assumptions, agree on a measurable outcome, and validate the result with users. Only use this if it is true for you.
