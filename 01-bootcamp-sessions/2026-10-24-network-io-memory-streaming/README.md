# 🌐 Session 05 — Network I/O, memory and streaming data

![Date](https://img.shields.io/badge/date-24%20Oct%202026-2563eb?style=flat-square)
![Type](https://img.shields.io/badge/type-Lecture%20%2B%20Q%26A-9333ea?style=flat-square)
![Questions](https://img.shields.io/badge/interview%20questions-8-16a34a?style=flat-square)
![Drill](https://img.shields.io/badge/drill-15%20min-f59e0b?style=flat-square)

> [!NOTE]
> **📼 Source:** supplied VTT transcript `GMT20261024-022820_Recording.cutfile.20261024110604786.transcript.vtt`. Not stored in this repository.
>
> **🧪 What was taught:** the first class of the actual syllabus (Module 1: Python and Linux foundations), taught "the FDE way": every concept tied to a real problem instead of toy examples.
> - network I/O: URL vs API vs website, protocols and when to pick each, HTTP vs HTTPS vs HTTP/3
> - HTTP methods (GET, POST, PUT, PATCH, DELETE), status codes, the `requests` library with query parameters
> - memory: `del`, garbage collection, `gc.collect()`
> - an LLM as a black box that works on numbers, not text
> - reading a 10,000-page PDF without running out of memory: OCR, page-by-page processing, generators and iterators
> - `stream=True` for large network responses
>
> **Assignment (due next weekend):** install a Linux environment with Multipass (WSL is fine if you already have it) for the Linux module.
>
> OOP, exception handling and file I/O were listed but moved to the next class. The interview questions are **independent preparation**, built from the class topics and the Q&A.

---

## 📚 What the instructor covered

### 🧭 How the course changes from here

| Point | What was said |
|---|---|
| 🎯 Goal | Not every concept "from 0 to 100", but **which concept solves this problem**, then use it |
| 📈 Pace | Faster each class; Linux will not take a full day |
| 🧑‍💼 Consulting topic | One non-syllabus "consultant" topic each weekend (the first one, ISO, came on 25 Oct → [Session 06](../2026-10-25-exceptions-files-iso/README.md)) |

### 📮 URL, API, website

| Term | Class definition |
|---|---|
| URL | A **digital mailing address** |
| Website | A URL that returns HTML, CSS and images, which the **browser renders** |
| API endpoint | A URL that returns **data** (JSON, XML) for a program to use |
| Protocol | The **rules of the conversation** between the two sides |

### 🔌 Pick the protocol for the job

| Need | Protocol |
|---|---|
| Web pages and API data | HTTP / HTTPS |
| Real-time **two-way** messages (chat apps) | WebSocket |
| Live video between people | WebRTC (also UDP-based options) |
| Moving large files between computers | FTP / **SFTP** |
| Sending and receiving mail | SMTP / IMAP (mail is **not** real-time) |
| Turning a name like a domain into an IP address | DNS |
| Lightweight messages for smart-home / IoT devices | MQTT |

> "Defining the protocol at the start of solving a problem is one of the major things."

### 🔐 HTTP, HTTPS, HTTP/3

| Version | What the class said |
|---|---|
| HTTP | Plain text: anyone watching the network can read it |
| HTTPS | Same, plus **TLS encryption** after a handshake; an attacker sees only scrambled bytes |
| HTTP/1 and 2 | Several round trips before data flows: a network handshake (SYN, SYN-ACK, ACK), then the encryption handshake |
| HTTP/3 | Combines the handshake and encryption setup, so the connection is ready in **one round trip**; encryption is built in |
| Seeing it | DevTools → Network → response headers: `alt-svc: h3=":443"` means the site offers HTTP/3 |

**FDE point:** you still write `https://`. A 10-year-old customer system may only speak HTTP/2 or even plain HTTP; the client and server agree on the best version. If a server only speaks plain HTTP, calling it with `https://` fails.

### 📨 HTTP methods and status codes

| Method | Body? | Use |
|---|:---:|---|
| GET | ❌ | Retrieve data |
| POST | ✅ | Send data: create something, or a query with a body |
| PUT | ✅ | Replace the **whole** resource |
| PATCH | ✅ | Change **part** of it (`{"name": "Asha"}` leaves `batch` untouched) |
| DELETE | ❌ | Remove it |

Methods are a **convention for other teams**: you *can* do everything with GET and POST, but using POST to read data confuses whoever calls your API.

| Result | Meaning |
|---|---|
| 2xx | Success |
| 4xx | **Client** error (wrong path → 404) |
| 5xx | **Server** error (busy, broken) |
| No status at all | The server was never reached (connection error) |

```python
import requests

params = {"limit": 1, "skip": 5}                     # query parameters as a dict
response = requests.get("https://dummyjson.com/products", params=params)
print(response.status_code)                          # 200
data = response.json()                               # fails if the body isn't JSON
```

The live demo hit a URL from outdated documentation that returned no JSON (`JSONDecodeError`), until the class found the right path. A useful lesson on its own: **check what the API actually returns before parsing it.**

### 🧠 Memory and garbage collection

| Idea | Class explanation |
|---|---|
| `del a` | Removes the **name**; the object may still be referenced (`b = a`) |
| Kept memory | Python keeps freed memory for reuse instead of returning it to the operating system |
| `gc.collect()` | Ask Python to clean up now. Rarely needed on a big server; consider it on a small one (4 GB RAM) with heavy jobs |

### 🤖 LLM as a black box

Treat an LLM as **an API that returns data**: input in, output out. Your job is what you send and what you do with the result. Internally, the model works on **numbers**, not text. (The class called these numbers embeddings; see the accuracy note.)

### 📄 The 10,000-page PDF problem

| Step | Point |
|---|---|
| 🧾 What's inside | Some pages have real text; others are **images of text** (scans). A plain PDF text library can't read images |
| 👁️ OCR | Optical character recognition turns pixels back into text: Tesseract, PaddleOCR, cloud services |
| 💾 Memory | A 2 GB file can take several times that in Python memory if read in one go |
| 🔁 Fix | Read and process **page by page**, with a generator that `yield`s one page at a time |

```python
def extract_pages(pdf_document):                      # pseudocode from class
    for page_number, page in enumerate(pdf_document, start=1):
        text = page.extract_text() or ""             # OCR here if the page is an image
        yield page_number, text                       # one page in memory at a time

pages = extract_pages(book)
first = next(pages)                                   # runs until the first yield, then pauses
```

| Term | Class explanation |
|---|---|
| Generator | A function with `yield`; it pauses after each value and resumes on `next()` |
| Iterator | The "parent" idea; a generator is a special, easier kind of iterator |
| Lazy built-ins | `range`, `enumerate`, `readline` give values one at a time |
| When it **doesn't** help | If the full data is already in a Python variable, yielding it piece by piece saves nothing |

### 🚿 `stream=True`

`requests.get(url, stream=True)` doesn't pull the whole body into Python at once. The bytes wait at the operating system's network layer until you read them piece by piece. **Trade-off:** slightly more latency, much less memory. "There is always a trade-off."

### 🎙️ From the cohort Q&A (summarised, no participant details)

| | Point |
|:---:|---|
| 🛰️ | **Polling many systems on a schedule** (dozens of clusters every 5 minutes): network-bound work suits threads or async. Run it once, measure the minimum and maximum time, then decide on concurrency and queues |
| 🔄 | **Prompt drift:** a new model can behave differently with the same prompt. Test before switching. If the old model scores 9/10 on your problem and the new one 8/10, keep the old one. **Version prompts per model** |
| ☕ | **Java vs Python:** Java can call LLM APIs; the class said the wider AI ecosystem (embeddings and so on) lives in Python. See the accuracy note |
| 🖼️ | **Images and video:** platforms store several resolutions in advance and switch between them; they don't convert on the fly |
| 🏭 | **Live defect detection:** for generic cases, a vision-language model can return bounding boxes from a text prompt. For specific or messy scenes, train your own detection model |
| 💸 | **Cheapest demo hosting:** exposing your own machine's ports is risky. A small cloud instance for a few hours costs very little |
| 📄 | **Small vs large documents:** a short document can go straight into the prompt. A large one needs chunking, embeddings and a vector database (RAG) |
| 🧑‍💼 | **"FDE" means different things at different companies.** Some want pre-sales experience, some want a pure IC. Target the role and pay you want, not the title |

---

## 🎯 Accuracy notes (independent, checked against the specs and Python behaviour)

> [!WARNING]
> The live class simplified several points. **Use the precise version in interviews:**
>
> - 🔢 **LLMs read and write token IDs, not embeddings.** Text is split into tokens (integer IDs). The model turns them into vectors internally, outputs a probability for each possible next token, and the chosen tokens are decoded back to text. The "embeddings" you store for RAG come from a separate **embedding model**.
> - 🌐 **There is no `http3://` scheme.** You always write `https://`. The browser learns from the `Alt-Svc` header that HTTP/3 is available. HTTP/3 runs over **QUIC on UDP** and uses TLS 1.3. A new connection needs one round trip; a resumed one can send data immediately ("0-RTT", with replay risks). HTTP/1.1 and HTTP/2 run over TCP: one round trip for TCP, plus one for TLS 1.3 (two for TLS 1.2).
> - 🔒 **Use HTTPS everywhere, even for a personal blog.** Certificates are free (Let's Encrypt), browsers mark plain HTTP as "Not secure", and encryption also stops content being changed in transit. "Inside our network we don't need it" is the opposite of **zero trust**: regulated customers often require encryption (even mutual TLS) between internal services.
> - 📮 **The same URL can return HTML or JSON.** It depends on the request (for example, the `Accept` header). "API vs website" is about who consumes the response, not the address.
> - 🔁 **Idempotency is the real difference between methods.** GET, PUT and DELETE are idempotent (repeating them has the same effect); POST is not, which is why retrying a POST can create duplicates. Some APIs legitimately use POST for complex searches (GraphQL, search with a large filter body).
> - 🧹 **CPython frees most objects immediately, by reference counting.** When the last reference goes, the object is freed. `del a` only removes one reference (and small integers like `10` are cached anyway). `gc.collect()` exists for **reference cycles**. Freed memory usually goes back to Python's own allocator for reuse, not to the operating system, so Task Manager may not drop even after `gc.collect()`.
> - 🔄 **`range` is a lazy *iterable*, not an iterator.** That's why `next(range(5))` failed in class. `iter(range(5))` gives an iterator. `enumerate`, `zip`, `map`, file objects and generators **are** iterators. Calling a generator function runs **none** of its body: it returns a generator object, and the code runs on each `next()`. A generator can be consumed only once and can't be indexed; use `itertools.islice` to skip ahead.
> - 📄 **For files, `for line in f:` is the lazy way.** `readlines()` loads every line into a list. `readline()` reads one line per call, but it isn't a generator.
> - 🚿 **`stream=True` is a client-side setting; the server doesn't change behaviour.** It defers downloading the body; unread bytes sit in the socket buffer and TCP flow control slows the sender. Read it with `response.iter_content(chunk_size=...)` or `iter_lines()`. `response.json()` or `.content` loads everything anyway. Use `with requests.get(..., stream=True) as r:` so the connection is released.
> - 📡 **For one-way live updates, prefer Server-Sent Events (SSE) over WebSocket.** LLM token streaming and live scores are server → client only; SSE is plain HTTP. Large-audience live video usually uses HLS/DASH (a few seconds of delay); WebRTC is for low-latency calls.
> - 🏷️ **MQTT** stands for Message Queuing Telemetry Transport. **Amazon Textract** is AWS's document OCR service (Rekognition is general image analysis).
> - ☕ **Java does have a solid LLM ecosystem:** Spring AI, LangChain4j and official Java SDKs from model providers. Python leads for ML research, training and notebooks. For an enterprise Java shop, adding an AI service in Java is a valid choice.

## ⏱️ 10-minute recap

Almost every FDE problem moves data **across a network** and **through memory**. This class gave the two questions to ask first:

| Question | Why it matters |
|---|---|
| **How does the data travel?** | Protocol, HTTPS, request method, and how failures show up (status code or no response) |
| **How much of it is in memory at once?** | Whole file vs page by page, whole response vs stream |

In customer work they look like this:

| Idea | Where an FDE meets it |
|---|---|
| Protocol choice | Live dashboard (WebSocket/SSE) vs nightly batch (HTTP or SFTP) |
| Status codes | Retry 5xx and 429; fail fast on 400 and 401 |
| Idempotency | Retries that must not create a duplicate ticket or payment |
| Generators | Large document ingestion with flat memory |
| `stream=True` | Downloading exports bigger than the server's RAM |
| Model behaviour | Pin the model version; re-test before upgrading |

## ⚖️ Key decisions and trade-offs

| Need | Choose | ⚠️ Watch out |
|---|---|---|
| Server pushes updates to a browser | SSE (one-way) or WebSocket (two-way) | WebSockets need sticky connections and reconnect logic |
| Partner sends nightly files | SFTP or object storage drop | Plain FTP sends passwords in clear text |
| Large file or response | Stream and process in chunks | `.read()`, `.json()` or `.content` undo it |
| Iterate over a big source | Generator | Single-use; no indexing |
| Many network calls in a fixed window | Threads or `asyncio`, with a cap | No cap = overload the target or yourself |
| Memory keeps growing | Measure with `tracemalloc` first | `gc.collect()` rarely fixes a real leak |
| New model release | Evaluate on your own cases first | "Newer" isn't always better for your task |

---

## 💬 Q1. A customer wants a live dashboard of factory alerts on their operators' screens. Which protocol would you use, and why?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "First I'd ask what 'live' means: within a second, or within a minute? And does the screen only receive alerts, or do operators also send actions back? If it only receives, I'd use Server-Sent Events: plain HTTPS, easy through corporate proxies, and the browser reconnects by itself. If operators acknowledge alerts in real time, a WebSocket fits, because it's two-way. If a one-minute delay is acceptable, simple polling every 30 seconds is the cheapest and easiest to support. Behind the dashboard, the machines themselves would probably publish over MQTT, which is built for lightweight devices. Whatever I choose, the screen must show clearly when it's disconnected, so nobody trusts a stale view."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why not just use WebSocket everywhere?</b></summary>

It adds work: load balancers must keep long-lived connections, you write reconnect and resubscribe logic, and some corporate proxies block it. If the data only flows one way, SSE gives you most of the benefit with less to operate.

</details>

<details>
<summary><b>What happens when the connection drops for 10 minutes?</b></summary>

The client reconnects and asks for everything **since the last event ID** it received (SSE supports a `Last-Event-ID` header for exactly this). Alerts are stored on the server, so nothing is lost; the screen shows "reconnecting" meanwhile.

</details>

<details>
<summary><b>How would the partner send you their daily production files?</b></summary>

Not over the live channel. SFTP or a drop into object storage (such as S3) with a checksum file. Large batch files and live alerts are different problems with different protocols.

</details>

---

## 💬 Q2. The customer's 10-year-old internal service only speaks plain HTTP. Do you insist on upgrading it?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "Not as a blocker on day one, but I'd raise it as a risk. Plain HTTP means anyone on that network path can read and change the data, including tokens. My first move is to put encryption in front of it without touching the old code: a reverse proxy or service mesh that terminates TLS, so new clients only ever talk HTTPS. Then I'd check whether the data is sensitive and what their security policy requires. Many regulated customers require encryption even inside the network. I'd write the decision down with the customer's security owner, because it's their risk to accept, not mine to ignore."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>What does HTTPS actually protect?</b></summary>

Confidentiality (nobody can read it in transit), integrity (nobody can change it unnoticed) and server identity (the certificate proves you reached the real server). It does **not** protect data once it arrives, or stop a logged-in user from misusing access.

</details>

<details>
<summary><b>What's the difference with HTTP/3?</b></summary>

It's still HTTPS from your point of view. Underneath, it runs over QUIC on UDP and sets up the connection and encryption together in one round trip, so it's faster on slow or lossy networks. Client and server agree on the version; old servers simply keep using HTTP/1.1 or HTTP/2.

</details>

<details>
<summary><b>The call fails with an SSL error. What do you check?</b></summary>

Whether the server speaks HTTPS at all (calling a plain-HTTP server with `https://` fails), whether the certificate is expired or self-signed, and whether a corporate proxy re-signs traffic with its own certificate authority. Fix it by adding the customer's CA bundle, **never** by turning verification off (`verify=False`) in production.

</details>

---

## 💬 Q3. You're designing a small API for a customer's tickets. How do you choose methods and status codes?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I follow the conventions so other teams can guess the behaviour. GET `/tickets/42` reads, POST `/tickets` creates, PATCH changes a few fields, PUT replaces the whole ticket, DELETE removes it. For status codes, 201 on create, 400 for invalid input, 401 when not logged in, 403 when logged in but not allowed, 404 when it doesn't exist, 409 on a conflict, 429 when rate-limited, and 5xx only for our own failures. The point is that a caller can decide what to do from the code alone: retry a 503, but fix the request on a 400. Because POST isn't idempotent, I accept an idempotency key so a retried create doesn't make two tickets."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>PUT or PATCH for changing a ticket's status?</b></summary>

PATCH: you send only `{"status": "closed"}`. PUT means "here is the complete ticket"; any field you leave out may be wiped. That's a common source of accidental data loss.

</details>

<details>
<summary><b>How does a client tell "server said no" from "server never answered"?</b></summary>

A status code means the server answered. A connection error or timeout means there's **no** status at all. These are different exceptions in `requests` (`HTTPError` vs `ConnectionError`/`Timeout`) and need different handling: a timeout on a create might still have succeeded, so check before retrying.

</details>

<details>
<summary><b>Is it ever right to use POST to read data?</b></summary>

Yes, when the query is too large or complex for a URL, for example a search with a long filter, or GraphQL. Document it clearly. Using POST for a simple read just because "it works" confuses everyone who calls the API.

</details>

---

## 💬 Q4. "We want to chat with our 10,000-page manual." Some pages are scanned. How do you build the ingestion?

> [!TIP]
> **🗣️ Say this (≈75 s)**
>
> "First I'd agree what success means: which questions users ask, and how they'll check an answer. Usually that means page citations. Then I'd look at the PDF itself, not assume. From my open-source work on pypdf I know a PDF page may have no text layer at all, or a broken internal structure. So I sample pages: if text extraction comes back empty, the page is a scan and goes to OCR. I process the document page by page with a generator, so memory stays flat. For each page I store the text, page number and how it was extracted, then chunk, embed and index it. I save progress as I go, so a failure on page 4,213 doesn't lose the first 4,212, and one bad page is logged and skipped instead of crashing the job. Finally I test on 20 to 30 real questions and check that the cited pages actually contain the answer."

> [!IMPORTANT]
> **✍️ Rav:** pick one of your merged pypdf pull requests about a malformed or unusual PDF and be ready to explain it in two sentences: what broke, and how you made pypdf handle it.

**❓ Follow-ups** · *click to open*

<details>
<summary><b>How do you detect which pages need OCR?</b></summary>

Per page: empty or very short extracted text, or text that is mostly garbage characters. Only those pages go to OCR, which is slower and costs more. Store the method per page, because OCR text is less reliable and you may want to show that.

</details>

<details>
<summary><b>Why not put the whole manual in the model's context?</b></summary>

It won't fit, and even when large context windows allow it, every question would pay for the whole manual in cost and latency. Models also tend to miss details in very long inputs. A short document can go straight into the prompt; a 10,000-page manual needs retrieval.

</details>

<details>
<summary><b>What else goes wrong with real PDFs?</b></summary>

Tables lose their structure, two-column pages come out in the wrong reading order, headers and footers repeat on every page and pollute search, and malformed files raise errors. Wrap each page in its own error handling, log the page number, and continue.

</details>

<details>
<summary><b>How do you show the customer it works?</b></summary>

A small evaluation set of real questions with the expected page. Report how often the right page was retrieved and how often the answer was correct. On a set of 30 questions, say it's a small sample, not a guarantee.

</details>

---

## 💬 Q5. A document service's memory grows until the server kills it. A developer added `del` and `gc.collect()`. It didn't help. Why, and what do you do?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "`del` only removes a name. If anything else still refers to the object, like a cache, a list of results or a global, it stays alive. `gc.collect()` only cleans up reference cycles, and even freed memory usually stays with Python for reuse instead of going back to the operating system, so the graph may not drop. I'd measure first: `tracemalloc` snapshots to see which lines allocate the most, and check what's being kept between requests. The usual fixes are design fixes: process in pages or chunks instead of loading whole files, stream large responses, bound any cache, and run heavy jobs in a worker process that exits or is recycled after N jobs, so the memory really goes back to the system."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>When is calling <code>gc.collect()</code> reasonable?</b></summary>

After building and dropping a big structure with reference cycles, in a long-running worker on a small machine, as one measured step. Not as a fix for a leak you haven't found.

</details>

<details>
<summary><b>Why can a 2 GB file need much more than 2 GB in Python?</b></summary>

Every Python object has overhead, a list of millions of small strings stores a pointer plus an object for each, and strings with non-ASCII characters can use 2 or 4 bytes per character. Measure it on a sample before choosing the server size.

</details>

<details>
<summary><b>What's "worker recycling"?</b></summary>

Servers like Gunicorn and task queues like Celery can restart a worker process after it handles a set number of requests or tasks. It's a safety net that caps the damage of slow memory growth while you find the root cause.

</details>

---

## 💬 Q6. You must pull a 5 GB export from a partner API onto a server with 4 GB of RAM. How?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "Never load it in one go. I'd stream it: `requests.get` with `stream=True`, write chunks straight to disk with `iter_content`, and verify the size or checksum at the end. Then I'd process the file in a streaming way too: line by line if it's newline-delimited JSON or CSV, or with a streaming JSON parser if it's one huge JSON array. Before building any of that, I'd ask the partner whether they offer pagination, an incremental 'changes since' endpoint, or a file drop. Downloading 5 GB every day when 50 MB changed is the real problem to solve."

```python
import requests

with requests.get(url, stream=True, timeout=(5, 60)) as r:
    r.raise_for_status()
    with open("export.ndjson", "wb") as f:
        for chunk in r.iter_content(chunk_size=1024 * 1024):   # 1 MB at a time
            f.write(chunk)
```

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Where is the data while you're not reading it?</b></summary>

In the operating system's socket buffer. When it fills, TCP tells the sender to slow down. Your Python process holds only the current chunk.

</details>

<details>
<summary><b>What if the download breaks at 4 GB?</b></summary>

If the server supports `Range` requests, resume from the byte you reached. If not, download to a temporary file and only rename it once it's complete and verified, so a half file is never processed as if it were whole.

</details>

<details>
<summary><b>Does a generator help if the code does <code>data = r.json()</code> first?</b></summary>

No. The whole response is already in memory; yielding it piece by piece saves nothing. Laziness has to start **at the source**.

</details>

---

## 💬 Q7. Every 5 minutes, a script pulls alerts from dozens of clusters and opens tickets. Runs now take longer than 5 minutes. What do you do?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "First I measure: how long each cluster call takes, typical and worst case, and whether one slow cluster holds up the rest. Calls one after another add up. Since this is network waiting, not CPU work, I'd run the calls concurrently, with threads or `asyncio`, capped at a sensible number, and give every call a timeout so one dead cluster can't stall the run. I'd stop runs overlapping with a lock, so a slow run can't start a second copy. Tickets get an idempotency key, such as the alert ID, so a repeated run never opens duplicates. And I'd ask whether the clusters can **push** alerts by webhook instead, which removes the polling problem altogether."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Threads, async or multiprocessing?</b></summary>

Waiting on the network: threads or `asyncio`; both let one process wait on many calls at once. Heavy CPU work, like parsing huge payloads: multiprocessing, because of Python's GIL (global interpreter lock). Here it's network waiting.

</details>

<details>
<summary><b>What if a cluster is down for an hour?</b></summary>

Its call times out quickly, the run continues for the others, and you raise one "cluster unreachable" alert instead of failing silently or opening a ticket every 5 minutes.

</details>

<details>
<summary><b>How do you know the fix worked?</b></summary>

Log the duration of every run and every call. Alert if a run takes more than, say, 80% of the window. The trend tells you before it breaks again.

</details>

---

## 💬 Q8. A newer, stronger model is released. Do you switch the customer's production feature to it?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "Not until it passes our own tests. A prompt tuned for one model can behave differently on another, which the class called prompt drift. I keep an evaluation set of real cases for each feature, run the new model against it, and compare quality, cost and latency. If the current model gets 9 of 10 right on our cases and the new one 8, we stay, even if the new one wins public benchmarks. Prompts are versioned together with the model they were tested on, and the model version is pinned in config. If the new model wins, I roll it out to a small share of traffic first and watch before switching fully. In my Aaina product, an evaluation harness runs 15 customer profiles; that's how I caught prompt-only rules breaking on 3 of them."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why pin the model version?</b></summary>

A floating alias like "latest" can change underneath you. With a pinned version, behaviour only changes when you decide, after testing.

</details>

<details>
<summary><b>What do you do when the old model is being retired?</b></summary>

Start the comparison early, as soon as the retirement date is announced. Re-tune the prompt for the new model against the same evaluation set, and keep the results as evidence for the customer.

</details>

<details>
<summary><b>Is 10 cases enough?</b></summary>

For a first check, it's a signal, not proof. Grow the set with real failures from production, and say the sample size whenever you report a score.

</details>

---

## 🏋️ Practical drill (15 minutes)

**Part A: predict.** Without running it, write down each output (or the error):

```python
def pages():
    print("start")
    for n in range(1, 4):
        yield n

g = pages()
print("created")
print(next(g))
print(list(g))
print(list(g))
print(next(iter(range(5))))
print(next(range(5)))
```

**Part B: match the protocol.** Pick one for each: HTTPS, WebSocket, SSE, SFTP, MQTT, WebRTC.

1. 5,000 soil sensors on a farm send a reading every minute
2. A bank sends a 2 GB reconciliation file every night
3. A web app shows LLM answers word by word as they're generated
4. Two support agents and a customer on a live video call
5. A shared whiteboard where everyone's edits appear for everyone instantly

<details>
<summary>🔑 <b>Answers</b> (open after trying)</summary>

**Part A:**

| Line | Output | Why |
|:---:|---|---|
| `g = pages()` | *(nothing)* | Calling a generator function runs none of its body |
| `print("created")` | `created` | |
| `next(g)` | `start`, then `1` | The body runs until the first `yield` |
| `list(g)` | `[2, 3]` | Continues from where it paused |
| `list(g)` again | `[]` | A generator is used up after one pass |
| `next(iter(range(5)))` | `0` | `iter()` makes an iterator from the range |
| `next(range(5))` | `TypeError` | `range` is an iterable, not an iterator |

**Part B:** 1 MQTT · 2 SFTP · 3 SSE · 4 WebRTC · 5 WebSocket (two-way, real-time). HTTPS sits underneath several of these, and is the answer for an ordinary API call.

</details>

## 🧾 Candidate evidence to prepare

- 📄 A pypdf pull request about a malformed PDF → [Open source in the story bank](../../03-story-bank/README.md)
- 🧪 Aaina's evaluation harness (15 profiles) as your "evaluate before changing the model" example
- ☕ Your Spring Boot + LLM API work at the bank as the answer to "Can Java do AI?"

> [!IMPORTANT]
> **✍️ To verify:**
> - which model versions your products pin today, and how you'd re-test them
> - whether any of your services stream large responses or files, and how

## 🔍 Open questions

- OOP, exception handling and file I/O moved to the next class → covered in [Session 06](../2026-10-25-exceptions-files-iso/README.md)
- The event loop, `asyncio`, threading and multiprocessing are next. Come back and deepen Q7 with real code
- Linux module starts next weekend: install Multipass or WSL before it

---

⬅️ [Session 04 — Control flow and functions](../2026-10-18-python-control-flow-functions/README.md) · ➡️ [Session 06 — Exceptions, files and ISO](../2026-10-25-exceptions-files-iso/README.md) · 🏠 [README](../../README.md)

🔗 Related: [AI systems](../../02-question-banks/ai-systems.md) · [FDE fundamentals](../../02-question-banks/fde-fundamentals.md) · [Story bank](../../03-story-bank/README.md)
