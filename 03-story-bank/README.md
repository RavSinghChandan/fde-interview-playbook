# Story bank

Keep each story truthful and concise. In a live interview, ask permission to go deeper (“I can explain the evaluation design if useful”). Replace placeholders only with facts you can verify.

## 1. Poultry 360 — user context, product judgment, accessibility

- **User/problem:** Farmers need bird counts and feed guidance without relying on manual counting and guesswork.
- **Constraints:** Plain CPU inference, low-literacy users, multiple Indian languages, and advice that must be grounded in trusted tables.
- **Your work:** YOLO11 exported to ONNX, FastAPI and Angular product, multilingual and read-aloud interaction, cited NRC/BIS tables, multi-tenant operations panel.
- **Evidence:** Evaluation found only 5 of 13 Hinglish trouble reports routed correctly (the rest were treated as plain lookups). After the routing fix it reached 13/13 on that in-sample set. A separate guard flags any invented number.
- **Trade-off to explain:** What performance or model capability was accepted to support CPU deployment? Add your measured results.
- **Also answers:** environment constraints (CPU-only hosting). See [Session 01](../01-bootcamp-sessions/2026-10-10-dev-environment-uv/README.md).

**Likely probes, with answers:**

- **Did you do user research?** Answer only with what you actually did (talks with farmers, field visits, feedback from users). If you did no formal research, say so: "I started from known problems in small broiler farms (hand counting, guessed feed) and published feed tables. Structured farmer interviews are my next step." *Rav: fill in your real evidence.*
- **What if the farmer's photo is poor?** The count comes from a vision model, so dark, blurry or crowded photos lower accuracy. The app should ask for a clearer photo rather than return a confident wrong number. Honest gap: bird-count accuracy is not yet measured on labelled shed photos. That is the next evaluation.
- **How do you keep the advice current?** Feed numbers come only from the published NRC 1994 / BIS IS 1374 tables shipped in the image, never from the model. Updating the advice means updating the tables and re-running the evaluations.
- **What happens without internet or AI?** Every path has a table or rule answer, so the farmer still gets the number when the AI key is missing or slow, just without the explanation. The UI is served from a CDN, so it opens even when the API server is waking up.
- **Privacy?** Photos are processed in memory and not stored. Registration keeps only what is needed to send a key (name, phone or email, district). Secrets live only in the hosting environment.
- **How are farms kept apart (tenant isolation)?** Every protected call carries a signed token that names the farm (an HMAC key), and sign-in is required for anything that spends CPU or the AI key.
- **What if the model invents a number?** A grounding guard checks every figure in the model's reply against the table. A figure that is not in the table is flagged, and the table figure is shown.
- **How were the 13 cases chosen, and what does "fixed" mean?** They were the Hinglish trouble reports from a 40-question English/Hindi/Hinglish routing set that runs in CI. "Fixed" means 13/13 on that same set after the change. That is in-sample, written and fixed by me, so it shows the bug is gone, not that routing is perfect in the field.
## 2. Aaina — evaluation, user preference, invariant enforcement

- **User/problem:** Salon consultation across 11 languages.
- **Your work:** Seven agents organized as a YAML-defined DAG; evaluation harness; code-level protection for customer dislikes; one repair retry.
- **Evidence:** Live evaluations found preference violations in 3 of 15 answers; the harness reached 15/15 on the tested set after the change.
- **Trade-off to explain:** Why code enforcement plus one repair retry was suitable; what the retry costs; what happens if it still fails.
- **Likely probes, with answers:**
  - **Why multiple agents?** Each step has its own schema and checks, so a failure is caught and repaired at that step. Independent steps run in parallel, which cuts latency. The ops panel also shows which agent is weak.
  - **How did you avoid unnecessary orchestration?** Agents and workflows are YAML run by one DAG engine. A step exists only when it has its own input, output and check. Rules that must always hold (the allowed catalog, the customer's dislikes) live in code, not in extra agents.
  - **What was the evaluation set?** 15 live customer profiles that I wrote myself. Prompt-only rules failed on 3 of them. With rules enforced in code, all 15 passed. That is in-sample, so I say it openly.
  - **What does the repair retry cost, and what if it still fails?** One extra model call, only when a check fails. A second failure falls back to an offline rule, so the customer in the chair always gets three valid cuts, with bounded latency and cost.
  - **Untrusted input and unsafe recommendations?** The catalog is narrowed to what this customer allows before the model sees it, and the answer is checked against the same rules afterwards. A model cannot choose an option it was never given. The customer's phone number never reaches the model, and no photo is analysed without consent.
## 3. Aura with Rav — safety and human control

- **User/problem:** Consumer AI product that produces personalized answers.
- **Your work:** LangGraph pipeline (12 nodes, 5 domain agents in parallel, then a synthesis agent) with prompt-injection checks, an output check, and human approval before a report is generated.
- **Evidence:** a 4-layer guard stack:
  1. Input check: regex patterns that block known injection phrases before any model call.
  2. Hardened system prompts. Each agent sees only its own state fields.
  3. Output check: system-prompt leaks, off-topic output, signs the model followed a jailbreak, and removal of harmful phrases.
  4. An audit log of every model call.
  Plus G1–G5: rate limiting, a circuit breaker, JSON repair, a PII output filter, and graceful degradation.
- **Likely probes, with answers:**
  - **What does each guard catch?** Layer 1 catches known injection phrasing ("ignore all previous instructions", "reveal your system prompt"). Layer 3 catches a leaked system prompt, off-topic output and fear-inducing claims. G4 blocks any insight that repeats the user's birth date, time or place.
  - **What can bypass it?** Regex catches known phrasing, not every rewording or another language, so it is a first filter, not a guarantee. That is why output checks and a human reviewer sit behind it.
  - **How do you minimize sensitive data?** Birth details are validated, and the PII filter stops them from being echoed into insights. The audit log stores an input hash and the output length, not the raw text.
  - **What happens when a reviewer disagrees?** The admin approves insights one by one, and only approved insight IDs go into the report. Rejected ones are dropped.
  - **Known limits, said openly:** the readiness review found that pipeline sessions are held in memory (a restart loses a run waiting for review), and that admin metrics are not filtered by tenant. Both are listed fixes.
## 4. Banking AI microservices — enterprise integration

- **Context:** LLM APIs integrated into Spring Boot services and event-driven AI microservices using Kafka in a regulated banking environment.
- **CV result:** 40% system-efficiency improvement is listed. Prepare the precise baseline, metric, time window, and attribution before using this claim.
- **Likely probes, with answers (the patterns are general; fill in only what you personally did):**
  - **Why events (Kafka)?** They decouple services: slow AI calls do not block the user's request, consumers can scale on their own, and the event log lets you replay after a failure.
  - **Idempotency?** Give each message a unique key and record processed keys, so a redelivered message does not apply twice (for example, a payment).
  - **Retries?** Retry with backoff for temporary errors. After a limit, send the message to a dead-letter topic for investigation instead of blocking the partition.
  - **Ordering?** Kafka keeps order only inside a partition, so partition by the entity (account ID) when order matters.
  - **Schema change?** Make backward-compatible changes (add optional fields), version the schema, and update consumers before producers start relying on new fields.
  - **Auditability and data protection?** Log who called which model with what kind of data. Mask or avoid personal data in prompts, and follow the bank's rules on what may leave its network.
  - **How was the 40% measured?** *Rav: write down the baseline, the metric, the time window, and what share was due to your change. Do not use the number until you can.*
- **Confidentiality:** Explain patterns and your own decisions without revealing employer or customer secrets.

## 5. Airline services — scale and reliability

- **Context:** Java microservices for airline systems processing 500K+ transactions each day.
- **Your contribution:** Prepare one specific service, bottleneck, failure mode, and decision that you personally owned.
- **Likely probes, with answers (general patterns; attach your own example):**
  - **Throughput vs latency?** Throughput is how many transactions per second, latency is how long one takes. Batching raises throughput but can hurt latency. Say which one your service cared about.
  - **Peak load?** 500K a day averages about 6 per second, but peaks can be many times that. Explain how the service handled bursts: queues, autoscaling, connection pools.
  - **Consistency?** Know which data had to be strongly consistent (bookings, payments) and which could be eventually consistent (reports, notifications).
  - **What if a downstream service is down?** Timeouts, retries with backoff, a circuit breaker, and a fallback or queued work so the failure does not spread.
  - **Duplicate messages?** Idempotent processing with a unique transaction key.
  - **Monitoring?** Error rate, latency percentiles (p95/p99), queue lag, plus alerts tied to user impact.
  - **Rollout and rollback?** Staged or canary releases, feature flags, and a tested rollback path. Name what your team actually used.
## 6. Open source — learning and collaboration

- **Evidence:** 106 merged pull requests across projects including pypdf and fontTools, reviewed and merged by maintainers.
- **Prepare one PR deeply:** User problem, repository context, reproduction, implementation, tests, review feedback, final outcome.
- **Likely probes, with answers:**
  - **A review you disagreed with?** Explain your reasoning politely with evidence (a test or a reproduction), then accept the maintainer's decision, because it is their project. Use a real PR thread.
  - **How do you prioritize external work?** Choose real bugs with a clear reproduction, keep a few PRs open at a time, and respond to reviews quickly.
  - **How did maintainers' standards change you?** You now read the project's conventions first, copy the nearest existing test style, keep changes small, and cover every new branch with a test before pushing.
- **Also answers:** "Can you write production Python?" See [Session 02](../01-bootcamp-sessions/2026-10-11-python-fundamentals/README.md).

## Story capture card

For each story, record: exact context; your role; collaborators; constraints; options; decision; test/evidence; outcome; what you would change. Mark anything confidential and create a safe, generalized interview version.
