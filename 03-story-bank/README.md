# Story bank

Keep each story truthful and concise. In a live interview, ask permission to go deeper (“I can explain the evaluation design if useful”). Replace placeholders only with facts you can verify.

## 1. Poultry 360 — user context, product judgment, accessibility

- **User/problem:** Farmers need bird counts and feed guidance without relying on manual counting and guesswork.
- **Constraints:** Plain CPU inference, low-literacy users, multiple Indian languages, and advice that must be grounded in trusted tables.
- **Your work:** YOLO11 exported to ONNX, FastAPI and Angular product, multilingual and read-aloud interaction, cited NRC/BIS tables, multi-tenant operations panel.
- **Evidence:** Evaluation caught Hinglish sick-flock reports routed incorrectly in 5 of 13 cases; routing and invented-number guard were fixed, reaching 13/13 on that evaluation.
- **Trade-off to explain:** What performance or model capability was accepted to support CPU deployment? Add your measured results.
- **Likely probes:** User research; poor photos; advice freshness; offline behavior; privacy; tenant isolation; model failure; how 13 cases were selected; what “fixed” means.

## 2. Aaina — evaluation, user preference, invariant enforcement

- **User/problem:** Salon consultation across 11 languages.
- **Your work:** Seven agents organized as a YAML-defined DAG; evaluation harness; code-level protection for customer dislikes; one repair retry.
- **Evidence:** Live evaluations found preference violations in 3 of 15 answers; the harness reached 15/15 on the tested set after the change.
- **Trade-off to explain:** Why code enforcement plus one repair retry was suitable; what the retry costs; what happens if it still fails.
- **Likely probes:** Why multiple agents? How did you avoid unnecessary orchestration? What was the evaluation set? How do you handle untrusted inputs and unsafe recommendations?

## 3. Aura with Rav — safety and human control

- **User/problem:** Consumer AI product that produces personalized answers.
- **Your work:** LangGraph pipeline with prompt-injection checks, hallucination check, and human approval before release.
- **Evidence to prepare:** Exact threat model, test cases, approval interface, measured failure behavior, and what still needs improvement.
- **Likely probes:** What does each guard catch? What can bypass it? How do you minimize sensitive data? What happens when a reviewer disagrees?

## 4. Banking AI microservices — enterprise integration

- **Context:** LLM APIs integrated into Spring Boot services and event-driven AI microservices using Kafka in a regulated banking environment.
- **CV result:** 40% system-efficiency improvement is listed. Prepare the precise baseline, metric, time window, and attribution before using this claim.
- **Likely probes:** Why events? Idempotency; retries; ordering; schema change; auditability; data protection; failure handling; how the improvement was measured.
- **Confidentiality:** Explain patterns and your own decisions without revealing employer or customer secrets.

## 5. Airline services — scale and reliability

- **Context:** Java microservices for airline systems processing 500K+ transactions each day.
- **Your contribution:** Prepare one specific service, bottleneck, failure mode, and decision that you personally owned.
- **Likely probes:** Throughput versus latency; peak load; consistency; downstream outage; retries and duplicate messages; monitoring; rollout and rollback.

## 6. Open source — learning and collaboration

- **Evidence:** 106 merged pull requests across projects including pypdf and fontTools, reviewed and merged by maintainers.
- **Prepare one PR deeply:** User problem, repository context, reproduction, implementation, tests, review feedback, final outcome.
- **Likely probes:** A review you disagreed with; how you prioritize external work; how maintainers' standards changed your approach.

## Story capture card

For each story, record: exact context; your role; collaborators; constraints; options; decision; test/evidence; outcome; what you would change. Mark anything confidential and create a safe, generalized interview version.
