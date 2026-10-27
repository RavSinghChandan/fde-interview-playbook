# 🛡️ Session 06 — Exceptions, files and ISO compliance

![Date](https://img.shields.io/badge/date-25%20Oct%202026-2563eb?style=flat-square)
![Type](https://img.shields.io/badge/type-Lecture%20%2B%20consulting%20topic%20%2B%20Q%26A-9333ea?style=flat-square)
![Questions](https://img.shields.io/badge/interview%20questions-8-16a34a?style=flat-square)
![Drill](https://img.shields.io/badge/drill-15%20min-f59e0b?style=flat-square)

> [!NOTE]
> **📼 Source:** supplied VTT transcript `GMT20261025-022945_Recording.transcript (2).vtt`. Not stored in this repository.
>
> **🧪 What was taught:**
> - exception handling: `try`/`except`/`else`/`finally`, specific vs general exceptions, nested handling, reading an error message, custom exceptions
> - an industry example: wrapping an LLM provider call (bearer token, timeouts, `raise_for_status`, 429 and 5xx)
> - file handling: `open`, closing, the `with` context manager, file modes, `read`/`readline`/`readlines`, `seek`/`tell`
> - **the first weekly consulting topic: ISO standards** (9001, 90003, 27001, 26262, plus SOC 2) and how a client's compliance requirement flows through a company
>
> OOP and concurrency moved to the next class. The interview questions are **independent preparation**, built from the class topics and the Q&A.

---

## 📚 What the instructor covered

### 🧯 Exception handling

| Part | Behaviour |
|---|---|
| No handling | `85 / 0` raises `ZeroDivisionError`; the script stops and nothing after it runs |
| `try` | The core logic that might fail |
| `except ZeroDivisionError:` | Runs only for that error type; add one block per type you handle differently |
| `except Exception as error:` | Catch-all, and `error` holds the message. **Keep it last**, or it hides the specific blocks |
| `else` | Runs only if `try` raised nothing (a good place for the `return`) |
| `finally` | **Always** runs, even after a `return`: used for clean-up (closing files and connections) |
| Nested | A `try`/`except` inside an `except`, when the fallback itself can fail |
| Reading an error | Type (`ZeroDivisionError`) + message (`division by zero`) |

```python
class InspectionLimitError(Exception):          # custom exception: name it, inherit Exception
    pass

if total_inspections < 2:
    raise InspectionLimitError("Daily mandatory inspection is required")
```

A business rule ("two inspections a day") is not a Python error, so you **define your own**.

### 🤖 Industry example: calling an LLM provider

| Step | Class version |
|---|---|
| Inputs | Provider URL, API key or token, prompt |
| Headers | `Authorization: Bearer <token>`, `Content-Type: application/json` |
| Call | `requests.post(url, headers=headers, json=payload, timeout=(3, 30))`: 3 s to connect, 30 s to read |
| Check | `response.raise_for_status()` raises `HTTPError` for any non-success status |
| Map the status | **429** → provider rate limit (common on free tiers) · **5xx** → provider server error · anything else → request rejected |
| Raise | `raise LLMProviderError(message) from error`, instead of only printing |

**Tokens:** the server checks who you are (ID, email, group) from the token before returning data. "Bearer" is a common prefix. Companies put LLM access behind the same kind of check.

**When to write `try`/`except`:** "Write the logic, test it, see what errors it actually raises, then handle those." See the accuracy note for the nuance.

### 📁 File handling

| Mode | Reads | Writes | Pointer starts | Wipes content | Creates file |
|:---:|:---:|:---:|---|:---:|:---:|
| `r` | ✅ | ❌ | Beginning | ❌ | ❌ |
| `w` | ❌ | ✅ | Beginning | ✅ | ✅ |
| `a` | ❌ | ✅ | End | ❌ | ✅ |
| `r+` | ✅ | ✅ | Beginning | ❌ | ❌ |
| `w+` | ✅ | ✅ | Beginning | ✅ | ✅ |
| `a+` | ✅ | ✅ (end only) | End | ❌ | ✅ |

| Method | Does |
|---|---|
| `read()` | Whole content |
| `readline()` | One line per call |
| `readlines()` | All lines as a list |
| `write()` / `writelines()` | Write a string / a list of strings |
| `tell()` | Where the pointer is |
| `seek(n)` | Move the pointer (`seek(0)` = start) |

- **Always close a file.** After closing, reading raises `ValueError: I/O operation on closed file`.
- **`with open(path, "r") as f:`** closes it automatically when the block ends. The class preferred this.
- The instructor's advice: "personally, never use `w`": one mistake wipes the file. `r+` (modify) and `a+` (add) are the two used most.
- Demo: find a report ID with `content.find("REP-")`, `seek` there and overwrite it with `r+`. The position was off by 2 because of special characters copied from a generated report (see the accuracy note).

### 🏛️ Consulting topic: ISO

**ISO** = International Organization for Standardization. It publishes standards: rules for how a company should **build, manage and secure** its products. They are **not coding rules**.

| Standard | Area | The client's question | What the team must do |
|---|---|---|---|
| **ISO 9001** | Quality management (certifiable) | "Can you deliver quality **consistently**?" | Defined requirements, code review, testing, change and defect tracking, controlled releases |
| **ISO/IEC/IEEE 90003** | Guidance for applying 9001 to **software** (not certifiable) | "How do your quality practices apply to software development and maintenance?" | Risk-based thinking before coding, defined baselines, configuration and testing |
| **ISO/IEC 27001** | Information security (certifiable) | "How do you manage information security risk?" | Access control, protected secrets and data, vulnerability management, documented risks |
| **ISO 26262** | Road-vehicle functional safety | "Can a software failure create a road hazard?" | Hazard analysis, traceable safety requirements, verification, evidence |
| **SOC 2** | Not ISO (an AICPA framework) | Often the first thing a startup is asked, especially by overseas clients | An independent audit of controls |

> "Very, very important: 27001 is the one most applicable to AI work. A client buying a RAG application will ask whether you follow it." ISO/IEC 42001 was also mentioned in passing.

A client can ask for **several** standards at once (for example 9001 and 26262). A company can **follow** a standard without being certified.

**How a client's ISO requirement flows through a company:**

| # | Who | What they do |
|:---:|---|---|
| 1 | **Client + sales/compliance** | "Our suppliers must meet ISO/IEC 27001." They clarify the exact scope, the contract terms and **what evidence they expect** |
| 2 | **Project manager + security/compliance lead** | Turn requirements into assigned tasks, policies, a risk assessment and acceptance criteria; name an **owner** for each requirement |
| 3 | **Architect + tech lead** | Decide what the system needs: authentication, role-based permissions, encryption, secure architecture, an audit-log pipeline |
| 4 | **Developers** | Implement code and infrastructure changes. **Data access is enforced in the API and data layer, never only in the front end** |
| 5 | **QA + security** | Try to break it: test access restrictions, inspect vulnerabilities, verify behaviour |
| 6 | **Project / compliance owner** | Keep the evidence: requirement records, code-review history, test reports, risk decisions, approvals, release records. Prepare for **internal and external audits** |

> "Is the job of an AI FDE just AI? No." An FDE may not do all of this alone, but should be able to join the conversation, translate requirements into tasks, and build the standard in from day one.

The instructor also noted that much AI work today is MVP or POC quality: thin code review, "AI reviewing AI". Real standards require more.

### 🎙️ From the cohort Q&A (summarised, no participant details)

| | Point |
|:---:|---|
| 🧱 | **Why a custom exception class?** Third-party APIs fail in ways Python's built-in exceptions don't describe; your own class gives callers a clear name and message to catch |
| 🤖 | **What is an agent?** A goal is split into a plan of steps, and each step is executed with a **tool** (for example, an email tool), instead of only answering a question |
| 🧰 | **Same model, different tool** (Copilot, Claude Code, Codex): the model can be the same; the interface around it differs |
| 🏗️ | **Configured platform agents vs custom builds:** a platform (the example was Salesforce Agentforce) is quick but closed; a custom build lets you choose chunking, overlap, embedding models and storage |
| 🧮 | **"AI everywhere":** a business asked for AI to fill null values that 2–3 deterministic rules in a SQL query could fill. A chatbot handling 300 rows per call over billions of rows turns minutes into hours. "First look for the problem, then see what can solve it" |
| 🔐 | **Company data and external LLMs:** use the company's enterprise agreement with the provider (data not used for training), or **self-host an open-weight model** such as Mistral, plus guardrails. Never paste company data into a personal or free API: it's a compliance issue |
| 🔌 | **Connecting an LLM to company data:** expose the data through an **API first**; MCP tools then call that API |
| 🧭 | **AI-assisted coding:** ask the AI for a plan, **read it and question it** before any code is written, and understand your project at the component level |
| 🕸️ | **Backend practice project (no AI):** a scraper for several websites at once, cleaning in the background, storage, and analysis on request. FastAPI for AI services; Django when you want built-in admin and user management |
| 🐞 | **Debugging:** VS Code breakpoints plus the debug console let you inspect any variable mid-run |
| ☸️ | **Kubernetes and scaling** are not in this course (they need real infrastructure spend); a separate system-design course covers them |

---

## 🎯 Accuracy notes (independent, checked against Python docs and the standards bodies)

> [!WARNING]
> The live class simplified several points. **Use the precise version in interviews:**
>
> - 🧱 **`try` needs at least one `except` *or* a `finally`.** `try`/`finally` without `except` is valid. A function can have many separate `try` blocks, and nesting is fine.
> - 🚫 **A bare `except:` is not the same as `except Exception:`.** Bare `except:` also catches `KeyboardInterrupt` and `SystemExit` (they inherit from `BaseException`), so Ctrl+C stops working. Avoid it.
> - ↩️ **`else` runs only when `try` raised nothing.** (At one point the class said the opposite.) A `return` inside `finally` overrides the earlier `return` *and silently swallows any exception*: never return from `finally`.
> - 🔗 **No import is needed for `Exception`;** it's built in. `raise NewError(...) from err` keeps the original error as the cause in the traceback. Build a small hierarchy (`class ProviderError(Exception)`, `class RateLimited(ProviderError)`) so callers catch by meaning.
> - 🧭 **"Handle errors only after you see them" is half right.** Explore first, yes. But failures you know will happen (network calls, file access, parsing user input) need handling designed up front, with tests for the error paths. Never wrap code in `try`/`except` just to hide errors.
> - ⏱️ **`timeout=(3, 30)` is not a 33-second total limit.** 3 s is the connect timeout; 30 s is the longest wait **between bytes** while reading. A slow trickle can run far longer. `requests` has no total-time limit; enforce a deadline yourself if you need one. Without `timeout`, `requests` can wait **forever**.
> - 🌐 **`raise_for_status()` raises for 4xx and 5xx only.** Connection failures and timeouts are different exceptions (`ConnectionError`, `Timeout`). Catch `requests.RequestException` to cover all of them. For 429, honour the `Retry-After` header and back off (see [Session 04, Q1](../2026-10-18-python-control-flow-functions/README.md)).
> - 🪪 **A bearer token is often a JWT: encoded and signed, *not encrypted*.** Anyone holding it can read its claims and use it, so treat it like a password and never log it. **Authentication** = who you are; **authorization** = what you may do.
> - 📝 **`w` isn't bad, but overwriting in place is risky.** The safe pattern is to write a temporary file in the same folder and then `os.replace()` it over the original, which is atomic. Mode `x` creates a file and fails if it already exists. Always pass `encoding="utf-8"` for text.
> - 📍 **The "+2" in the demo was an encoding issue.** `str.find()` returns a **character** index. In text mode, `seek()`/`tell()` work with opaque, byte-based positions, and a bullet like `•` takes 3 bytes in UTF-8. Also, `r+` **overwrites** bytes, it doesn't insert: a replacement of a different length corrupts the file. Real code reads, modifies in memory and rewrites the file safely.
> - ♻️ **CPython usually closes an unreferenced file object when it's garbage-collected,** but the timing isn't guaranteed (and other Python implementations differ). That's why `with` is the right habit, not because Python can't close it.
> - 🔢 **It's ISO/IEC/IEEE 90003:2018** (the class said "9003" / "90,003"). Current editions: ISO 9001:2015, ISO/IEC 27001:2022 (Annex A lists 93 controls).
> - 🤖 **ISO/IEC 42001:2023 is the AI management-system standard, and it is certifiable.** For an AI FDE it may matter as much as 27001: AI risk assessment, impact assessment, data and model lifecycle controls.
> - 🏷️ **SOC 2 means "System and Organization Controls 2"** (AICPA), not "Security Operations Center" (that's a monitoring team). It's an **attestation report** by an independent CPA firm, not a certificate. **Type I** checks the design of controls at one point in time; **Type II** checks that they operated over a period (often 6–12 months).
> - 🏢 **Certification covers a defined scope of an organisation's management system.** It's issued by an accredited certification body, not by ISO itself, and it isn't for a product or a person.
> - ⚖️ **Laws are different from standards.** HIPAA (US health data), GDPR (EU), India's DPDP Act 2023 and the EU AI Act are legal obligations. ISO standards and SOC 2 are frameworks you choose or a client requires by contract. ISO/IEC 27701 extends 27001 to privacy.

## ⏱️ 10-minute recap

Two halves of the same idea: **things go wrong, and you must be able to show how you handle it.**

| In code | In the organisation |
|---|---|
| Expected failures are caught by type and handled on purpose | Risks are written down, with an owner and a decision |
| A custom exception names a business rule | A policy turns a client requirement into a rule |
| `finally` / `with` always clean up | Controls always run, not only when someone remembers |
| Logs record what happened | Evidence (reviews, tests, approvals, releases) proves it to an auditor |

For an FDE, compliance is not paperwork after the build. It shapes the architecture: where access is checked, what is logged, where data may go and which model can be used.

## ⚖️ Key decisions and trade-offs

| Need | Choose | ⚠️ Watch out |
|---|---|---|
| Handle a failure you expect | `except SpecificError` | A broad `except` hides bugs |
| Clean up a resource | `with` (or `finally`) | Never `return` from `finally` |
| Signal a business rule | A custom exception | Long `if/print` chains callers can't catch |
| Call an external API | Timeouts + `raise_for_status` + mapped errors | No timeout = a request that can hang forever |
| Change a file safely | Write a temp file, then `os.replace` | `r+` overwrites; `w` wipes |
| Client asks for security assurance | Map the request to 27001 / SOC 2 scope | Promising "certified" when you only "follow" |
| AI product for an enterprise | Consider ISO/IEC 42001 as well | Treating AI risk as ordinary IT risk |
| Data can't leave the customer | Self-hosted or private-endpoint model | Self-hosting has real GPU and operations cost |

---

## 💬 Q1. Walk me through how you'd wrap a call to an LLM provider so failures are handled properly.

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I put the call in one small function that the rest of the code uses. It always sets a timeout, both to connect and to read. It calls `raise_for_status`, then turns each failure into my own exception with a clear meaning: rate-limited, provider down, request rejected, or network failure. Rate limits and server errors get a bounded retry with backoff, honouring `Retry-After`. Bad requests and auth errors fail at once, because retrying won't fix them. I chain the original error with `raise ... from`, log a request ID but never the API key or the full prompt if it holds personal data, and let the caller decide what the user sees. In my Aura product, that's where graceful degradation and the circuit breaker sit."

```python
import requests

class ProviderError(Exception): ...
class RateLimited(ProviderError): ...
class ProviderDown(ProviderError): ...

def call_llm(url, token, payload):
    try:
        r = requests.post(url, json=payload, timeout=(3, 30),
                          headers={"Authorization": f"Bearer {token}"})
        r.raise_for_status()
    except requests.HTTPError as err:
        code = err.response.status_code
        if code == 429:
            raise RateLimited("provider rate limit reached") from err
        if code >= 500:
            raise ProviderDown(f"provider returned {code}") from err
        raise ProviderError(f"request rejected ({code})") from err
    except requests.RequestException as err:            # connection errors, timeouts
        raise ProviderDown("provider unreachable") from err
    return r.json()
```

> [!IMPORTANT]
> **✍️ Rav:** check what Aura's G1–G5 guards actually do when the provider fails (G2 circuit breaker, G5 graceful degradation), and what the user sees, before using this example.

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why your own exception types instead of letting <code>HTTPError</code> through?</b></summary>

Callers shouldn't need to know HTTP or which provider library you use. `RateLimited` tells them what happened and what to do. If you switch provider or library, only this function changes.

</details>

<details>
<summary><b>What's wrong with <code>except Exception: print(error)</code>?</b></summary>

The program carries on as if nothing failed, with no result. The user sees nothing useful, monitoring sees nothing, and the real cause is lost. Catch what you can handle; let the rest go up to a layer that logs it and returns a proper error.

</details>

<details>
<summary><b>What does <code>raise ... from err</code> give you?</b></summary>

The traceback shows both errors: your meaningful one and the original cause. Debugging a production failure without the original cause is guesswork.

</details>

---

## 💬 Q2. How do you decide where to catch an exception, and where to let it go?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "I catch an exception where I can actually do something about it: retry, use a fallback, or turn it into a clear message. Everywhere else I let it rise. At the edge of the system, such as an API handler or a job runner, one handler logs the full error with context and returns a safe response, so nothing is swallowed silently. I catch specific types, never a bare `except`, and I use `with` for anything that must be cleaned up. And I test the error paths, not only the happy path."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>When are <code>else</code> and <code>finally</code> useful?</b></summary>

`else` keeps the `try` block small: only the risky line goes in `try`, and the follow-on work goes in `else`, so you don't accidentally catch errors from it. `finally` is for clean-up that must always happen. With files and connections, `with` usually does that job more neatly.

</details>

<details>
<summary><b>Why does the order of <code>except</code> blocks matter?</b></summary>

Python uses the first matching block. `except Exception` first would catch everything, and the specific handlers below it would never run. Specific first, general last.

</details>

<details>
<summary><b>Is it OK to catch an exception and continue a batch?</b></summary>

Yes, if you record it: in a 10,000-row import, log the failed row and the reason, continue, and report "9,987 imported, 13 failed (see list)". Skipping silently is never OK.

</details>

---

## 💬 Q3. A job must update one value inside a large config or report file. How do you do it safely?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "I don't edit the file in place. I read it, change the value in memory, write the result to a temporary file in the same folder, and then `os.replace` it over the original. That swap is atomic, so a crash halfway never leaves a half-written file. I always use `with` and an explicit `encoding='utf-8'`. If it's a structured format like JSON or YAML, I parse and write it with the proper library instead of searching for text positions. If the file is too big for memory, I stream it line by line into the temp file."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why not <code>seek</code> to the position and overwrite with <code>r+</code>?</b></summary>

Two traps. First, `r+` overwrites bytes rather than inserting, so a longer or shorter value corrupts what follows. Second, text positions and character positions differ as soon as there's a non-ASCII character. That's exactly why the class demo was off by two.

</details>

<details>
<summary><b>What if two processes update the file at the same time?</b></summary>

One will lose its change. Use a file lock or, better, move shared state that changes often into a database, which is designed for concurrent updates.

</details>

<details>
<summary><b>When would you use append mode?</b></summary>

For logs and audit trails, where you only ever add to the end. It's the safest write mode, because nothing earlier can be overwritten.

</details>

---

## 💬 Q4. A client says: "Our suppliers must meet ISO/IEC 27001." You're the FDE on the deal. What do you do?

> [!TIP]
> **🗣️ Say this (≈75 s)**
>
> "First I find out exactly what they need. Do they require us to be certified, or to show that we follow the controls? Which systems and data are in scope? What evidence do they expect, and when? That's a conversation with our compliance or security lead, not a promise I make alone. Then I map it to the architecture: who can access what, enforced in the API and data layer; encryption in transit and at rest; how secrets are stored; an audit log of access and changes; and how we handle vulnerabilities and incidents. Each requirement gets an owner and a test. As the build goes on, we keep the evidence: review history, test reports, risk decisions and release records. For an AI product I'd also raise what happens to their data at the model provider, and whether ISO/IEC 42001 matters to them."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>What's the difference between "certified" and "follows the standard"?</b></summary>

Certified means an accredited body audited a defined scope of our management system and issued a certificate. "Follows" means we've implemented the practices but no external audit confirmed it. Never say "certified" unless the certificate exists and covers the scope the client cares about.

</details>

<details>
<summary><b>What evidence would an auditor ask for?</b></summary>

A risk assessment and the decisions made on it, access reviews (who has access and who approved it), change records and code-review history, test and vulnerability-scan reports, incident records, and logs that prove the controls actually run.

</details>

<details>
<summary><b>What's your honest experience with this?</b></summary>

*Rav: fill in.* Answer pattern: "In the bank, I worked inside processes that existed for regulation: [reviews, approvals, access controls you saw]. I haven't led a certification myself. What I can do is build the controls into the system from day one and keep the evidence as we go."

</details>

---

## 💬 Q5. Explain ISO 9001, ISO/IEC 27001, ISO/IEC 42001 and SOC 2 to a non-technical stakeholder.

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "They answer different questions a client has about a supplier. ISO 9001 asks: do you deliver consistent quality, with reviews, testing and controlled releases? ISO/IEC 27001 asks: do you manage information security risk, like access, secrets and incidents? ISO/IEC 42001 is the newer one for AI: do you manage the specific risks of AI systems, like data, bias and model changes? SOC 2 is an American audit report, popular with SaaS buyers, where an independent auditor checks that your security controls actually operated over a period. None of them tells developers how to write code; they say what must be controlled and proven."

| | Asks | Result |
|---|---|---|
| 🧩 ISO 9001 | Consistent **quality** | Certificate |
| 🔐 ISO/IEC 27001 | Information **security** risk | Certificate |
| 🤖 ISO/IEC 42001 | **AI** risk and governance | Certificate |
| 🧾 SOC 2 | Security controls **working over time** | Auditor's report (Type I / Type II) |

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Where do laws like GDPR or India's DPDP Act fit?</b></summary>

Laws are mandatory wherever they apply. Standards are frameworks that help you meet them and prove it. A client in Europe may need both GDPR compliance and a 27001 certificate from you.

</details>

<details>
<summary><b>A startup client has none of these. Does it matter?</b></summary>

They can still follow the practices from day one: access control, reviews, tests, logs. It makes a later certification or SOC 2 audit much cheaper, and it's simply good engineering.

</details>

<details>
<summary><b>What's the difference between SOC 2 Type I and Type II?</b></summary>

Type I: are the controls designed properly, at one point in time? Type II: did they actually work over a period, often 6 to 12 months? Enterprise buyers usually want Type II.

</details>

---

## 💬 Q6. Where do you enforce who can see which data, and how would you prove it to an auditor?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "In the API and the data layer, never only in the front end. Anyone can call an API directly and skip the UI, so every request checks the user's identity and role on the server, and queries are filtered by what that role may see. I keep the rules in one place, deny by default, and test them, including a test that a user from tenant A gets nothing from tenant B. For proof, every access and change is written to an audit log: who, what, when and the outcome, append-only. In my Aura product, every model call is logged with a request ID, user, input hash, output length and cost, which is the same idea applied to AI calls."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Role-based or attribute-based access?</b></summary>

Role-based (admin, analyst, viewer) is simpler and enough for most products. Attribute-based (region, department, data sensitivity) is for finer rules. Start with roles, and add attributes only when a real rule needs them.

</details>

<details>
<summary><b>What goes wrong with access control in AI features?</b></summary>

A RAG system can leak documents the user isn't allowed to read if retrieval ignores permissions. Filter retrieved documents by the user's access **before** they reach the model, not after.

</details>

<details>
<summary><b>What must never go into the audit log?</b></summary>

Passwords, tokens, API keys and unnecessary personal data. That's why Aura logs an input **hash**, not the input itself.

</details>

---

## 💬 Q7. The customer wants an LLM feature, but their data must not leave their environment. What are your options?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "First I'd pin down what 'leave' means to their security team. Not stored by the provider? Not used for training? Not processed outside the country? Or never outside their own network? Each answer opens different options. A provider's enterprise terms with no training and no retention may be enough. A cloud deployment in their own account and region may be enough. If nothing may leave their network, we self-host an open-weight model, such as a Mistral model, on their infrastructure, and accept the GPU cost and that it may be less capable, so we test it on their real tasks first. In every case, I send the model only what it needs: redact personal data first. In Aaina, the customer's phone number never reaches the model."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>What does self-hosting really cost?</b></summary>

GPUs, or at least large CPU machines, plus the people to run them: scaling, monitoring, model updates and security patches. Compare that with the risk and the contract terms of a managed option before choosing.

</details>

<details>
<summary><b>A developer pasted customer data into a free AI tool to debug. What now?</b></summary>

Treat it as an incident: stop, report it to the security or compliance owner, and record what was shared and where. Then prevent repeats with approved tools and clear guidance, not blame.

</details>

<details>
<summary><b>How do you connect the model to their internal data?</b></summary>

Through an API with its own access checks, and expose that API to the model as tools (for example, over MCP). The model never gets direct database credentials.

</details>

---

## 💬 Q8. The business wants "an AI solution" for a job a SQL query can do. How do you handle it?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I start from what they actually need: correct results, on time, at a known cost. Then I show the options side by side on their real data: the SQL or script version with its runtime, cost and accuracy, and the AI version with the same numbers. If the rules are known and stable, code is faster, cheaper, testable and gives the same answer every time. Pushing billions of rows through a model in small batches turns minutes into hours. Usually there's a real place for AI nearby: explaining the results, handling messy free-text cases the rules can't, or letting people ask questions in plain language. I'd propose that instead, so they get visible AI value without making the core job worse."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>What if leadership insists on AI anyway?</b></summary>

I'd write down the trade-off (cost, time, accuracy) and the decision, build the safest version, and measure it against the code baseline so the data speaks. If it's my call, I recommend the baseline; if it isn't, the risks are visible.

</details>

<details>
<summary><b>When is AI the right tool for data cleaning?</b></summary>

When the input is free text or inconsistent: addresses, product descriptions, notes in mixed languages. Rules can't cover those well. Even then, validate the output with code and send unclear cases to a person.

</details>

<details>
<summary><b>Do you have an example of choosing code over AI?</b></summary>

Yes: in Aaina, rules that must always hold, like customer dislikes and the allowed catalog, are enforced in code, not left to the prompt. Prompt-only rules broke on 3 of 15 test profiles; with rules in code, 15 of 15 passed on that set → [story bank](../../03-story-bank/README.md).

</details>

---

## 🏋️ Practical drill (15 minutes)

**Part A: predict.** Without running it, write down what each call prints and returns:

```python
def f(x):
    try:
        result = 10 / x
    except ZeroDivisionError:
        print("zero")
        return -1
    else:
        print("ok")
        return result
    finally:
        print("cleanup")

print(f(2))
print(f(0))

def g():
    try:
        return "try"
    finally:
        return "finally"

print(g())
```

**Part B: compliance mapping.** For each client sentence, name the standard or law it points to:

1. "Show us your auditor's report proving your controls worked over the last year."
2. "Our patients' records are involved, and we operate in the US."
3. "How do you manage the risks of the AI models in your product?"
4. "We need proof you have a consistent release and defect process."
5. "Personal data of Indian users will be processed."

<details>
<summary>🔑 <b>Answers</b> (open after trying)</summary>

**Part A:**

| Call | Prints | Returns | Why |
|---|---|---|---|
| `f(2)` | `ok`, `cleanup` | `5.0` | No error → `else`, and `finally` runs before the return completes |
| `f(0)` | `zero`, `cleanup` | `-1` | `except` handles it; `finally` still runs |
| `g()` | *(nothing)* | `finally` | A `return` in `finally` overrides the earlier one (and would swallow an exception): avoid it |

**Part B:** 1 SOC 2 Type II · 2 HIPAA (a US law) · 3 ISO/IEC 42001 · 4 ISO 9001 · 5 India's DPDP Act 2023.

</details>

## 🧾 Candidate evidence to prepare

- 🧾 Aura's audit layer (request ID, user, input hash, output length, cost) and PII output filter → [Aura in the story bank](../../03-story-bank/README.md)
- 🛑 Aaina: phone number never sent to the model; rules enforced in code
- 🏦 The bank: what regulation changed about how you built, reviewed and released software

> [!IMPORTANT]
> **✍️ To verify:**
> - which compliance processes you personally took part in at the bank (reviews, approvals, access requests, audits), without naming confidential details
> - whether any of your live products would need a data-processing agreement with its model provider

## 🔍 Open questions

- OOP (the four pillars) and concurrency (`asyncio`, threading, multiprocessing) are next. Deepen [Session 05, Q7](../2026-10-24-network-io-memory-streaming/README.md) when they're taught
- Read the scope of ISO/IEC 42001 once; it's the standard most specific to AI products
- The Linux module starts next weekend (Multipass or WSL)

---

⬅️ [Session 05 — Network I/O, memory and streaming](../2026-10-24-network-io-memory-streaming/README.md) · 🏠 [README](../../README.md)

🔗 Related: [AI systems](../../02-question-banks/ai-systems.md) · [Customer discovery](../../02-question-banks/customer-discovery.md) · [Story bank](../../03-story-bank/README.md)
