# Session 00 — Bootcamp induction (4 October 2026)

**Source:** supplied VTT transcript, `GMT20261004-021909_Recording.transcript.vtt`
**Session type:** induction and cohort Q&A
**Also used:** the short same-day follow-up Q&A recording, `GMT20261004-084659_Recording.transcript.vtt` (see the last section).
**Technical lesson:** this recording does not teach a detailed technical implementation. Treat the interview prompts below as preparation derived from the session themes and your CV, not as verbatim instructor questions.

## Useful ideas from the session

- The bootcamp is planned for roughly five to six months; the instructor discussed extending access. Check your own dashboard for current access details.
- The teaching goal is not simply to learn a fixed list of tools. Apply concepts to your own domain and project.
- FDE work is framed around the customer and end-to-end product development. MLOps is more centered on operating and optimizing ML applications. The boundary can overlap in real teams.
- In the Q&A, the instructors stressed depth in core topics and being able to handle follow-up questions, rather than learning every technology superficially.
- One discussion highlighted validation as essential. A demo that runs is not enough; know how you would detect hallucination, failure, or a bad user outcome.
- The session is an induction, so do not treat cohort questions or examples as a final syllabus. Add later class material in its own folder.

## 10-minute recap

An FDE does not begin by picking a fashionable framework. They find the customer's real workflow, constraints, and success measure; build a practical solution; integrate and validate it; then help it work in the customer's environment. Your existing examples let you demonstrate this approach, especially Poultry 360 and your enterprise integration work.

## Likely interview questions

### 1. What does Forward Deployed Engineering mean to you?

**Short answer:** “It means taking engineering close to the customer's real problem. I work with the user to understand the workflow and constraints, build and integrate a useful solution, validate it with real cases, and stay accountable through deployment and feedback.”

**Follow-ups to rehearse:**

- How is that different from a product engineer? The FDE works more directly in a customer's context and often handles discovery and integration; product engineering may build for a broader, shared roadmap. In practice, the roles can overlap.
- How is it different from consulting? Consulting may end with recommendations. An FDE is expected to build, integrate, and prove the solution works, although organizations define the role differently.
- How is it different from MLOps? MLOps focuses on the lifecycle and operation of ML systems. FDE includes customer discovery and end-to-end workflow delivery; it may include MLOps when needed.
- How would you measure success? User adoption and task outcome first, then reliability, latency, cost, and support burden as appropriate.

### 2. Tell me about a time you turned an unclear need into a working product.

Use **Poultry 360**. Explain the farmer's task, why manual counting or generic advice was insufficient, the low-literacy and language constraints, your product choices, and how you evaluated the behavior. Be ready to explain how you validated the need with users; do not imply user research steps that you did not perform.

**Follow-ups:** How did you select the first feature? What was the least expensive useful version? What would you do if farmers had unreliable internet? How did you validate the advice? What would make you remove the AI component?

### 3. How do you stop yourself from building a demo instead of a product?

**Short answer:** “I define a real user task and success condition, test realistic and failure cases, integrate with the workflow, and observe what happens after launch. For my products, evaluations caught concrete failures such as Hinglish sick-flock reports being routed incorrectly and customer dislikes being dropped. I turned those findings into checks and fixes.”

**Follow-ups:** What cases were in the evaluation? How do you prevent test data leakage? What is the difference between a good offline score and a good customer outcome? What do you monitor after deployment?

### 4. How do you choose what to learn or build from a course?

**Short answer:** “I convert each concept into a decision I might face at work: what problem does it solve, when is it a poor fit, how will I test it, and what is the smallest implementation I can explain? I then connect it to one of my existing products rather than collecting notes on tools.”

**Follow-ups:** Show one example. How will you avoid shallow coverage? How do you keep interview preparation compatible with active applications?

## This week's practice (20 minutes)

1. Give the FDE definition in 45 seconds.
2. Explain one product using CLEAR from `00-start-here/answer-method.md`.
3. Ask yourself “why?”, “what failed?”, “how do you know?”, and “what would you change?” after every claim.
4. Note any metric you cannot defend and correct it before using the story.

## Same-day follow-up Q&A (afternoon recording)

A short career Q&A. No technical lesson. The instructors' points, summarized without participant details:

- **AI is usually an enhancement on top of existing software, not a replacement.** The examples were a phone camera that uses AI to improve photos, and a chat app that is not itself AI. Enterprise stacks such as Java are not going away.
- **"AI FDE" vs FDE.** Engineers who already own client-facing delivery may already be doing FDE work. The AI part is what the course adds.
- **The FDE route was described as sufficient for experienced engineers.** Architect roles focus more on design and guiding developers, and the right target depends on the job description.
- **Relevance in banking and insurance companies depends on the project.** Most companies are trying to add AI, so look for openings where you can apply it.

### Interview question: You come from Java and enterprise systems. Why FDE, and how does that experience help?

**Short answer:** "Most AI value in a company sits on top of systems that already exist: banking services, airline transactions, internal tools. I have built those systems in Java, so I understand integration, reliability and the constraints of regulated environments. I have also built and run AI products end to end. An FDE needs both: someone who can connect a model to a real workflow safely, inside the customer's existing stack."

**Follow-ups:** Which AI feature would you add to a system you built before, and how would you measure it? What would you refuse to automate? How do you handle data that cannot leave the customer's network?

**Honesty rule:** describe your past work as it was. Show how it prepares you for AI integration, but never relabel non-AI work as AI work.

## Open items for later sessions

- Add the official module or class topic once a technical session transcript is supplied.
- Replace assumptions about customer discovery with your exact evidence and experience.
- Record the bootcamp's actual project and evaluation requirements once taught.
