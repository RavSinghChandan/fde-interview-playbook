# A simple method for strong interview answers

Use **CLEAR** when answering a project, technical, or customer question:

1. **C — Context:** Who had the problem? What was happening?
2. **L — Limitation:** What made the problem hard? What constraints mattered?
3. **E — Engineering choice:** What did you choose and why? What did you not choose yet?
4. **A — Assurance:** How did you test, measure, secure, deploy, or monitor it?
5. **R — Result and reflection:** What changed? What would you improve next?

Keep the first answer to about one minute. Then let the interviewer probe. A good answer is not a speech; it is a clear starting point for a technical conversation.

## When you do not know

“I haven’t implemented that exact approach. My current understanding is ___. I would verify ___, then compare it against ___ using ___. In my own system, the closest related decision was ___.”

This is stronger than bluffing. Be precise about what you know and what you would investigate.

## For system design or customer cases

Start with the user and decision, not a tool name:

- Who is the user, and what are they trying to do?
- What happens today, and where does it fail or cost time?
- What systems, data, permissions, connectivity, language, or budget constraints exist?
- What is the smallest useful end-to-end slice?
- What can go wrong, and how will the user recover?
- What evidence would show the solution is useful and safe?
- How will we roll it out, train users, and support it?

Only then choose APIs, models, agents, retrieval, or infrastructure.
