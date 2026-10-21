# 📖 Story bank

*Six true stories, each with every likely probe answered.*

> [!TIP]
> Keep each story truthful and concise. In a live interview, offer depth instead of dumping it: *"I can explain the evaluation design if useful."* Replace placeholders only with facts you can verify.

| # | Story | Best for |
|:---:|---|---|
| 1 | 🐔 **Poultry 360** | User context, product judgment, accessibility |
| 2 | 💇 **Aaina** | Evaluation, user preference, rules enforced in code |
| 3 | 🔮 **Aura with Rav** | Safety, guardrails, human approval |
| 4 | 🏦 **Banking AI microservices** | Enterprise integration, events, regulation |
| 5 | ✈️ **Airline services** | Scale and reliability |
| 6 | 🔀 **Open source** | Learning, code review, Python depth |

---

## 🐔 1. Poultry 360

| | |
|---|---|
| 👩‍🌾 **User / problem** | Farmers need bird counts and feed guidance without hand counting and guesswork |
| 🚧 **Constraints** | CPU-only inference · low-literacy users · several Indian languages · advice must come from trusted tables |
| 🛠️ **Your work** | YOLO11 exported to ONNX · FastAPI + Angular · multilingual read-aloud · cited NRC/BIS tables · multi-tenant ops panel |
| 📊 **Evidence** | Only **5 of 13** Hinglish trouble reports routed correctly (the rest were treated as plain lookups). After the fix: **13/13** on that in-sample set. A separate guard flags any invented number |
| ⚖️ **Trade-off** | What model capability was accepted to run on CPU? *Add your measured results* |
| 🔗 **Also answers** | Environment constraints (CPU-only hosting) → [Session 01](../01-bootcamp-sessions/2026-10-10-dev-environment-uv/README.md) |

**❓ Likely probes** · *click to open*

<details>
<summary><b>Did you do user research?</b></summary>

Answer only with what you actually did (talks with farmers, field visits, user feedback). If there was no formal research, say so:

> "I started from known problems in small broiler farms (hand counting, guessed feed) and published feed tables. Structured farmer interviews are my next step."

✍️ *Rav: fill in your real evidence.*

</details>

<details>
<summary><b>What if the farmer's photo is poor?</b></summary>

Dark, blurry or crowded photos lower the vision model's accuracy. The app should **ask for a clearer photo** rather than return a confident wrong number.

⚠️ Honest gap: bird-count accuracy is **not yet measured on labelled shed photos**. That is the next evaluation.

</details>

<details>
<summary><b>How do you keep the advice current?</b></summary>

Feed numbers come only from the published **NRC 1994 / BIS IS 1374** tables shipped in the image, never from the model. Updating advice = update the tables + re-run the evaluations.

</details>

<details>
<summary><b>What happens without internet or AI?</b></summary>

Every path has a table or rule answer, so the farmer still gets the number when the AI key is missing or slow, just without the explanation. The UI is served from a CDN, so it opens even while the API server wakes up.

</details>

<details>
<summary><b>Privacy?</b></summary>

- 📷 Photos are processed in memory and **not stored**
- 📝 Registration keeps only what's needed to send a key (name, phone or email, district)
- 🔑 Secrets live only in the hosting environment

</details>

<details>
<summary><b>How are farms kept apart (tenant isolation)?</b></summary>

Every protected call carries a **signed token naming the farm** (an HMAC key). Sign-in is required for anything that spends CPU or the AI key.

</details>

<details>
<summary><b>What if the model invents a number?</b></summary>

A **grounding guard** checks every figure in the model's reply against the table. Any figure not in the table is flagged, and the table figure is shown.

</details>

<details>
<summary><b>How were the 13 cases chosen, and what does "fixed" mean?</b></summary>

They were the Hinglish trouble reports from a **40-question English/Hindi/Hinglish routing set** that runs in CI. "Fixed" = 13/13 on that same set after the change.

That's **in-sample** (written and fixed by me), so it shows the bug is gone, not that routing is perfect in the field.

</details>

---

## 💇 2. Aaina

| | |
|---|---|
| 💁 **User / problem** | Salon consultation (three suitable haircuts) across 11 languages |
| 🛠️ **Your work** | Seven agents as a YAML-defined DAG · evaluation harness · customer dislikes protected in code · one repair retry |
| 📊 **Evidence** | Prompt-only rules broke on **3 of 15** live profiles. With rules in code: **15/15** on the tested set |
| ⚖️ **Trade-off** | Why code enforcement + one repair retry; what the retry costs; what happens if it still fails |

**❓ Likely probes** · *click to open*

<details>
<summary><b>Why multiple agents?</b></summary>

Each step has its own schema and checks, so a failure is caught and repaired **at that step**. Independent steps run **in parallel**, which cuts latency. The ops panel shows which agent is weak.

</details>

<details>
<summary><b>How did you avoid unnecessary orchestration?</b></summary>

Agents and workflows are YAML run by **one DAG engine**. A step exists only if it has its own input, output and check. Rules that must always hold (allowed catalog, customer dislikes) live **in code**, not in extra agents.

</details>

<details>
<summary><b>What was the evaluation set?</b></summary>

**15 live customer profiles** I wrote myself. Prompt-only rules failed on 3; with rules in code, all 15 passed. That's in-sample, and I say so openly.

</details>

<details>
<summary><b>What does the repair retry cost, and what if it still fails?</b></summary>

**One extra model call**, only when a check fails. A second failure falls back to an **offline rule**, so the customer in the chair always gets three valid cuts, with bounded latency and cost.

</details>

<details>
<summary><b>Untrusted input and unsafe recommendations?</b></summary>

- The catalog is **narrowed before** the model sees it and the answer is **checked after**. A model can't choose an option it was never given.
- The customer's **phone number never reaches the model**.
- **No photo is analysed without consent.**

</details>

---

## 🔮 3. Aura with Rav

| | |
|---|---|
| 🙋 **User / problem** | Consumer AI product that produces personalised readings and reports |
| 🛠️ **Your work** | LangGraph pipeline: 12 nodes, 5 domain agents in parallel, then a synthesis agent · injection checks · output checks · **human approval** before a report is generated |
| 🛡️ **Guard stack** | See the table below, plus **G1–G5**: rate limiting · circuit breaker · JSON repair · PII output filter · graceful degradation |

| Layer | What it does |
|:---:|---|
| 1️⃣ Input | Regex patterns block known injection phrases before any model call |
| 2️⃣ Prompts | Hardened system prompts; each agent sees only its own state fields |
| 3️⃣ Output | Detects system-prompt leaks, off-topic output and jailbreak compliance; removes harmful phrases |
| 4️⃣ Audit | Every model call logged (request ID, user, input hash, output length, cost) |

**❓ Likely probes** · *click to open*

<details>
<summary><b>What does each guard catch?</b></summary>

- **Layer 1:** known injection phrasing ("ignore all previous instructions", "reveal your system prompt")
- **Layer 3:** a leaked system prompt, off-topic output, fear-inducing claims
- **G4:** any insight that repeats the user's birth date, time or place

</details>

<details>
<summary><b>What can bypass it?</b></summary>

Regex catches **known** phrasing, not every rewording or another language. It's a first filter, not a guarantee, which is why output checks and a **human reviewer** sit behind it.

</details>

<details>
<summary><b>How do you minimise sensitive data?</b></summary>

Birth details are validated, and the PII filter stops them being echoed into insights. The audit log stores an **input hash and output length**, not raw text.

</details>

<details>
<summary><b>What happens when a reviewer disagrees?</b></summary>

The admin approves insights **one by one**; only approved insight IDs go into the report. Rejected ones are dropped.

</details>

<details>
<summary><b>What are the known limits? (say them openly)</b></summary>

The readiness review found:
- pipeline sessions are held **in memory**, so a restart loses a run waiting for review
- admin metrics are **not filtered by tenant**

Both are listed fixes.

</details>

---

## 🏦 4. Banking AI microservices

| | |
|---|---|
| 🏢 **Context** | LLM APIs integrated into Spring Boot services, and event-driven AI microservices on Kafka, in a regulated bank |
| 📊 **CV result** | **40%** system-efficiency improvement is listed |

> [!IMPORTANT]
> **✍️ Rav:** before using the 40%, write down the **baseline, metric, time window and your share** of the change. The probe answers below are general patterns. Fill in only what you personally did.

> [!CAUTION]
> Explain patterns and your own decisions **without revealing employer or customer secrets**.

**❓ Likely probes** · *click to open*

<details>
<summary><b>Why events (Kafka)?</b></summary>

They **decouple services**: slow AI calls don't block the user's request, consumers scale on their own, and the event log lets you **replay** after a failure.

</details>

<details>
<summary><b>Idempotency?</b></summary>

Give each message a **unique key** and record processed keys, so a redelivered message doesn't apply twice (for example, a payment).

</details>

<details>
<summary><b>Retries?</b></summary>

Retry with **backoff** for temporary errors. After a limit, send the message to a **dead-letter topic** instead of blocking the partition.

</details>

<details>
<summary><b>Ordering?</b></summary>

Kafka keeps order **only inside a partition**, so partition by the entity (e.g. account ID) when order matters.

</details>

<details>
<summary><b>Schema change?</b></summary>

Make **backward-compatible** changes (add optional fields), version the schema, and update consumers before producers rely on new fields.

</details>

<details>
<summary><b>Auditability and data protection?</b></summary>

Log who called which model with what kind of data. **Mask or avoid personal data** in prompts, and follow the bank's rules on what may leave its network.

</details>

<details>
<summary><b>How was the 40% measured?</b></summary>

✍️ *Rav: baseline → metric → time window → what share came from your change. Don't use the number until you can answer this.*

</details>

---

## ✈️ 5. Airline services

| | |
|---|---|
| 🏢 **Context** | Java microservices for airline systems processing **500K+ transactions a day** |
| 👤 **Your contribution** | ✍️ *Prepare one specific service, bottleneck, failure mode and decision you personally owned* |

> [!NOTE]
> The probe answers below are general patterns. Attach your own example to each.

**❓ Likely probes** · *click to open*

<details>
<summary><b>Throughput vs latency?</b></summary>

**Throughput** = transactions per second; **latency** = time for one. Batching raises throughput but can hurt latency. Say which one your service cared about.

</details>

<details>
<summary><b>Peak load?</b></summary>

500K a day averages about **6 per second**, but peaks can be many times higher. Explain how bursts were handled: queues, autoscaling, connection pools.

</details>

<details>
<summary><b>Consistency?</b></summary>

Know what had to be **strongly consistent** (bookings, payments) and what could be **eventually consistent** (reports, notifications).

</details>

<details>
<summary><b>What if a downstream service is down?</b></summary>

Timeouts · retries with backoff · a **circuit breaker** · a fallback or queued work, so the failure doesn't spread.

</details>

<details>
<summary><b>Duplicate messages?</b></summary>

**Idempotent processing** with a unique transaction key.

</details>

<details>
<summary><b>Monitoring?</b></summary>

Error rate · latency percentiles (p95/p99) · queue lag · alerts tied to user impact.

</details>

<details>
<summary><b>Rollout and rollback?</b></summary>

Staged or canary releases, feature flags and a tested rollback path. Name what your team actually used.

</details>

---

## 🔀 6. Open source

| | |
|---|---|
| 📊 **Evidence** | **122 merged pull requests** across projects including pypdf and fontTools, reviewed by maintainers |
| 🎯 **Prepare one PR deeply** | User problem → repository context → reproduction → implementation → tests → review feedback → outcome |
| 🔗 **Also answers** | "Can you write production Python?" → [Session 02](../01-bootcamp-sessions/2026-10-11-python-fundamentals/README.md) · "Why test you when agents write code?" → [Session 04](../01-bootcamp-sessions/2026-10-18-python-control-flow-functions/README.md) |

**❓ Likely probes** · *click to open*

<details>
<summary><b>A review you disagreed with?</b></summary>

Explain your reasoning politely **with evidence** (a test or a reproduction), then accept the maintainer's decision, because it's their project. Use a real PR thread.

</details>

<details>
<summary><b>How do you prioritise external work?</b></summary>

Pick real bugs with a clear reproduction, keep a few PRs open at a time, and respond to reviews quickly.

</details>

<details>
<summary><b>How did maintainers' standards change you?</b></summary>

You now **read the project's conventions first**, copy the nearest existing test style, keep changes small, and cover every new branch with a test before pushing.

</details>

---

## 🗃️ Story capture card

Copy this for each new story:

| Field | Your notes |
|---|---|
| 📍 Exact context | |
| 👤 Your role | |
| 🤝 Collaborators | |
| 🚧 Constraints | |
| 🔀 Options considered | |
| ✅ Decision | |
| 🧪 Test / evidence | |
| 📈 Outcome | |
| 🔁 What you'd change | |

> [!CAUTION]
> Mark anything confidential and create a safe, generalised interview version.

---

🏠 [Back to README](../README.md)
