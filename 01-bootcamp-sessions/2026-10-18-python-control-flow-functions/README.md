# 🔀 Session 04 — Python recap, part 3: control flow, loops and functions

![Date](https://img.shields.io/badge/date-18%20Oct%202026-2563eb?style=flat-square)
![Type](https://img.shields.io/badge/type-Python%20recap%20%2B%20Q%26A-9333ea?style=flat-square)
![Questions](https://img.shields.io/badge/interview%20questions-6-16a34a?style=flat-square)
![Drill](https://img.shields.io/badge/drill-15%20min-f59e0b?style=flat-square)

> [!NOTE]
> **📼 Source:** supplied VTT transcript `GMT20261018-022830_Recording.cutfile.20261018100319680.transcript.vtt`. Not stored in this repository.
>
> **🧪 What was taught:**
> - `if`/`elif`/`else`, nesting, truthiness, conditional expressions
> - `for` and `while` loops, `range`, `break`/`continue`, nested loops, dry runs
> - looping over dicts, `enumerate`, unpacking
> - functions: parameters vs arguments, `return`, positional/keyword/default arguments, `*args`/`**kwargs`
> - variable scope and `global`
>
> **Self-study before the next class:** exception handling, file handling, the `os` module, and OOP basics. The course syllabus, starting with OOP pillars, begins next week.
>
> The interview questions are **independent preparation**, built from the class topics and the Q&A.

---

## 📚 What the instructor covered

### 🧭 Control flow

| Form | Behaviour |
|---|---|
| `if` alone | Runs its block only when the condition is true |
| `if … else` | One block; `else` takes no condition and can't exist without an `if` |
| `if`, `if`, `if` | **Separate** checks; several can run |
| `if … elif … elif … else` | **One** block; only the **first** true branch runs, so order matters (`>= 90`, then `>= 70`, then `>= 50`) |
| Nested `if` | Club entry: check age first, and only then ask for ID |
| Conditional expression | `status = "adult" if age >= 18 else "minor"` for short choices only |

**Truthiness:** `False`, `None`, `0`, `0.0`, `""`, and empty `[]`, `()`, `{}`, `set()` all count as false. Anything else, including the string `"False"`, counts as true.

### 🔁 Loops

| Topic | Key point |
|---|---|
| Iterable | Something you can take items from one at a time (a bag of marbles): str, list, tuple, set, dict, `range`. An `int` is not |
| `range(start, end, step)` | End is exclusive, like slicing. `range(1, 11)` is 1–10; `range(1, 11, 2)` is odd numbers |
| `for i in iterable:` | `i` is just a variable that receives each item; any name works, and it still exists after the loop |
| `continue` | Skip the rest of **this** iteration and fetch the next item |
| `break` | Leave the **innermost** loop; the outer loop carries on |
| `pass` | Does nothing; a placeholder |
| Nested loops | The inner loop runs fully for each outer item. A `break` inside it stops only the inner loop |
| `while condition:` | Use when the end isn't known in advance. Change the variable inside the loop or `break`, otherwise it never ends |
| Dicts | `for k in d` gives keys; `for k, v in d.items()` gives pairs |
| `enumerate` | `for i, (k, v) in enumerate(d.items())` adds a counter from 0, replacing a manual `count += 1` |

**Dry run:** step through the code on paper, writing down each variable's value, before reaching for print statements or a debugger. "This is what senior developers do."

**While-loop example from class:** an app that stores user data must get **consent** (a compliance rule, not a company preference). Keep asking with a friendly interval until the user agrees, rather than nagging with constant pop-ups.

### 🤖 Functions (the "robot" analogy)

| Idea | Analogy | Code |
|---|---|---|
| Function | A robot that runs named instructions | `def calculate_total(price, quantity):` |
| Parameters | The robot's **hands** | `price`, `quantity` |
| Arguments | Values you hand over | `calculate_total(60, 70)` |
| `return` | The robot hands back the answer sheet; `print` only shows it | `value = calculate_total(60, 70)` |

| Argument kind | Example | Rule |
|---|---|---|
| Positional | `add(10, 2)` | Matched by order |
| Keyword | `add(b=3, a=2)` | Matched by name; order doesn't matter |
| Default | `def greet(name, message="Great day")` | **Defaults must come after non-defaults**, or the call would be ambiguous |
| `*args` | `def total(*args): return sum(args)` | Extra positional values arrive as a **tuple** |
| `**kwargs` | `def show(name, phone, **kwargs)` | Extra keyword values arrive as a **dict** (`email=...`) |
| Order | `def f(*args, **kwargs)` | Positional before keyword |

### 🔭 Variable scope

| Case | Result |
|---|---|
| `a = a + 1` inside a function, `a` only global | ❌ `UnboundLocalError` |
| A local `a` defined inside the function | The function uses its own `a`; the global is unchanged |
| `global a` before changing it | The function changes the global |
| Only **reading** a global | ✅ allowed |
| `a.append(x)` on a global list | ✅ works: the object changes, the name isn't reassigned |

### 📝 Self-study set for the next class

| Priority | Topics |
|---|---|
| 1️⃣ Before next class | Exception handling (≈20 min) · `os` module: `os.path.join`, `os.getcwd`, `os.listdir`, relative vs absolute paths · file handling **with** exception handling · OOP basics: class, object, constructor, methods, attributes (the pillars will be taught in class) |
| 2️⃣ Within two weeks | List and dict comprehensions · built-ins `any`, `all`, `map`, `filter`, `enumerate`, `zip`, `sorted` · `lambda` |

### 🎙️ From the cohort Q&A (summarised, no participant details)

| | Point |
|:---:|---|
| 🧪 | **Practice:** HackerRank-style graded problems; mentors don't grade the assignments, they are for self-checking |
| 📏 | **Evaluation** of RAG and agents will be taught inside projects (tools such as DeepEval and RAGAS were mentioned), not as a separate theory block |
| 🧑‍🍳 | **FDE vs specialist:** an Agentic-AI specialist is the chef with 10 years in one cuisine; the FDE can cook across cuisines and builds the **whole system** around the AI part |
| 🧰 | **FDE vs subject-matter expert (SME):** an SME brings domain knowledge; an FDE is an **engineer who orchestrates and builds** end to end |
| 🚦 | **Don't let AI make production deployment or scaling decisions.** A wrong scaling choice can cost a lot of money in a month, and someone has to be accountable. Automate with scripts and CI/CD, but humans approve |
| ⌨️ | **Coding interviews still matter** even with coding agents. Agents write the boilerplate; you check the logic and the edge cases, for example a missing `try/except` around an external API that will break the day that API goes down |
| 🏦 | **The FDE loop:** understand the customer's problem → choose the technology on cost and latency (ML, LLM or plain code) → POC → build → run on the **customer's existing infrastructure** → monitor |
| 🧷 | **Typed-language background:** Python supports type hints, but they aren't enforced; it trades strictness for speed of building |
| 🕰️ | **"Is FDE a fad?"** The role existed before (startup leads who coded, managed and met clients); AI made the breadth easier to reach |

---

## 🎯 Accuracy notes (independent, checked against Python behaviour)

> [!WARNING]
> The live recap simplified a few points. **Use the precise version in interviews:**
>
> - 🔐 **Python encapsulation is by convention, not enforcement.** The class quiz left this open. There is no `private`: `_name` means "internal, please don't touch", `__name` triggers **name mangling** (to `_Class__name`) to avoid clashes in subclasses, and `@property` gives controlled access. That's the real difference from Java.
> - 🏷️ **`*args` collects extra *positional* arguments; `**kwargs` collects extra *keyword* arguments.** At one point the class called `**kwargs` positional. Also useful: parameters after a bare `*` are **keyword-only**, and parameters before `/` are **positional-only**.
> - 🧩 **`match`/`case` (Python 3.10+) is structural pattern matching**, not just a switch, and it isn't reliably faster than `if/elif`. Choose it for readability when matching shapes of data.
> - 🔁 **`for … else` exists:** the `else` runs only when the loop finished **without** `break`. It's a neat "not found" branch.
> - 🔢 **`enumerate(items, start=1)`** starts counting from 1, so you don't need `i + 1`.
> - 🌐 **A consent screen is not a `while` loop on the server.** In a web app a blocking loop would freeze a worker. The real design is a **state check**: the backend refuses to store data until a consent record exists, and the front end decides when to ask again. The `while` idea fits a CLI or a retry loop.
> - 🧱 **`global` is rarely the right fix.** In a web service, a global changed by one request is visible to every other request and thread. Pass values in and `return` results instead.
> - 🗂️ **Modern code prefers `pathlib`** (`Path.cwd() / "data" / "file.csv"`) to `os.path.join`, but read both, since older codebases use `os`.

## ⏱️ 10-minute recap

Control flow decides **which** lines run; loops decide **how many times**; functions give a block a **name, inputs and an output**. In customer code the same three ideas show up as:

| Idea | Where an FDE meets it |
|---|---|
| `if/elif` order | Business rules: the most specific condition must come first |
| `while` + `break` | Retrying a flaky API with a **maximum** number of attempts |
| `for` + `continue` | Skipping bad rows while counting them |
| `return`, not `print` | Results other code (and tests) can use |
| Defaults and `**kwargs` | Flexible wrappers that can also **hide typos** |
| Scope | Shared global state leaking between requests |

## ⚖️ Key decisions and trade-offs

| Need | Choose | ⚠️ Watch out |
|---|---|---|
| Known number of items | `for` | Changing a list while looping over it |
| Unknown end (retry, poll) | `while` + a cap + `break` | No cap = infinite loop or runaway cost |
| Many exclusive cases | `if/elif/else` | Wrong order shadows later branches |
| Optional settings | Default parameters | Never a mutable default (see [Session 02](../2026-10-11-python-fundamentals/README.md)) |
| Forwarding options | `**kwargs` | Typos pass silently; validate the keys |
| Share state | Arguments and return values | `global` leaks across requests |
| Missing value check | `if x is None` | `if not x` also rejects `0` and `""` |

---

## 💬 Q1. Your service calls a partner API that sometimes fails. How do you call it safely?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I wrap the call in a bounded retry. A loop runs at most three to five attempts, waits longer each time, with a little randomness so clients don't retry in sync, and only retries errors that can be temporary: timeouts, connection errors, 429 and 5xx. A 400 or 401 won't fix itself, so it fails at once with a clear error. Every call has a timeout. After the last attempt I log the failure with a request ID and either return a fallback or raise an error the caller handles. If failures continue, a circuit breaker stops calling the partner for a while so we don't pile on."

```python
import random, time

def call_with_retry(fetch, attempts=4, base=0.5):
    for attempt in range(1, attempts + 1):
        try:
            return fetch()                    # fetch() sets its own timeout
        except TemporaryError:                # timeouts, 429, 5xx only
            if attempt == attempts:
                raise
            time.sleep(base * 2 ** (attempt - 1) + random.uniform(0, 0.1))
```

> [!IMPORTANT]
> **✍️ Rav:** your Aura story lists a circuit breaker and graceful degradation (G1–G5). Be ready to say which calls they protect and what the user sees when they trigger.

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why not <code>while True</code> until it works?</b></summary>

If the partner is down for an hour, every request hangs, workers run out, and if the call is paid, the cost keeps growing. A cap turns an outage into a fast, visible error.

</details>

<details>
<summary><b>What if the call creates something, like a payment?</b></summary>

Retrying can create it twice. Use an **idempotency key** the partner honours, or check whether the first attempt actually succeeded before retrying.

</details>

<details>
<summary><b>How do you know the retry policy is right?</b></summary>

Log attempts per call and the final outcome. If most calls succeed on attempt 2, that's fine; if many need attempt 4, the partner has a real problem to raise with them.

</details>

---

## 💬 Q2. The customer's app stores user data. How do you make sure consent is recorded before anything is stored?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I treat consent as data the backend enforces, not just a pop-up. The server refuses to store personal data unless there's a consent record for that user: which version of the terms they agreed to, when, and for which purposes. The front end decides when to ask, kindly, without blocking everything else that doesn't need personal data. When the terms change, the version check asks again. Withdrawing consent stops new processing and starts the agreed deletion. I'd confirm the exact rules with the customer's legal team, for example India's DPDP Act or GDPR for EU users, rather than guess."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why not a loop that keeps showing the terms until they agree?</b></summary>

On a server, a waiting loop freezes a worker. In the UI, nagging is a bad experience and in some places isn't valid consent. The backend check gives the guarantee; the UI only chooses when to ask.

</details>

<details>
<summary><b>What do you store as proof?</b></summary>

User ID, terms version, timestamp, purposes, and how it was given (which screen). Store it append-only, so the history can't be quietly rewritten.

</details>

<details>
<summary><b>How does this connect to AI features?</b></summary>

Personal data should not reach a model the user didn't agree to. In my Aaina product, no photo is analysed without consent, and the customer's phone number never reaches the model.

</details>

---

## 💬 Q3. Should an AI agent be allowed to deploy to production or scale infrastructure by itself?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "Not without a human approval for anything that costs money or touches production. An agent can prepare the change: write the script, open the pull request, show the plan and the cost estimate. A person approves, and the CI/CD pipeline applies it. The reasons are accountability, because 'the AI did it' isn't an answer to a customer, and the size of the damage, because a wrong scaling decision can cost thousands in a month. I built the same pattern into Aura with Rav: the agents draft insights, and an admin approves them one by one before a report is generated."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>What can be automated safely?</b></summary>

Read-only and easy-to-reverse steps: running tests, linting, building images, preparing plans, deploying to a staging environment. Add hard limits too, such as a budget alarm and a maximum number of instances, so even approved automation can't run away.

</details>

<details>
<summary><b>Doesn't human approval slow everything down?</b></summary>

Only on the risky steps. Low-risk changes can be pre-approved by policy. The point is to put people where a mistake is expensive, not everywhere.

</details>

<details>
<summary><b>What's the weakness in your Aura approval flow?</b></summary>

Pipeline sessions are held in memory, so a restart loses a run that is waiting for review. It's a known, listed fix. I say so openly → [story bank](../../03-story-bank/README.md).

</details>

---

## 💬 Q4. Coding agents write most code now. Why should we test your coding at all?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "Because I'm responsible for what ships, not the agent. I use coding agents to go faster on boilerplate, but I read every line before it merges. I check the logic, the edge cases, error handling around external calls, and whether it follows the codebase's conventions. My open-source work is good practice for this: maintainers review every line of my pull requests, and they find exactly the things a generator misses, like an untested branch or a name that breaks the project's pattern. If I can't explain the code, I'm not ready to merge it."

> [!IMPORTANT]
> **✍️ Rav:** prepare one true example where you caught a bug in generated code or a reviewer caught one in yours, with the PR or commit you can show.

**❓ Follow-ups** · *click to open*

<details>
<summary><b>How do you review AI-generated code?</b></summary>

- run it with the real input and one bad input
- look for missing timeouts, bare `except:`, swallowed errors
- check that the tests actually exercise the new branches, not only the happy path
- compare with the two nearest existing examples in the codebase

</details>

<details>
<summary><b>What do coding agents get wrong most often?</b></summary>

Invented library functions, quietly changed behaviour outside the request, missing edge cases, and tests that confirm the code instead of the requirement.

</details>

<details>
<summary><b>Would you use one in this interview?</b></summary>

Only if allowed. Either way I'd explain my reasoning aloud, because the interviewer is assessing judgment.

</details>

---

## 💬 Q5. A teammate's function takes `**kwargs` and passes them on. A customer setting "isn't working," with no error. What might be wrong?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "My first guess is a typo or a renamed option. `**kwargs` accepts any name, so `time_out=30` instead of `timeout=30` is silently ignored or dropped. I'd log the keys actually received, then fix the design: name the supported options explicitly, often as keyword-only parameters after a `*`, and raise an error on unknown keys if forwarding is truly needed. Type hints and a test that passes the setting and checks its effect stop it coming back."

```python
def connect(host, *, timeout=10, retries=3):   # keyword-only, explicit
    ...

connect("db", time_out=30)   # ❌ TypeError at once, instead of being ignored
```

**❓ Follow-ups** · *click to open*

<details>
<summary><b>When is <code>**kwargs</code> the right choice?</b></summary>

Thin wrappers and decorators that must forward whatever the wrapped function accepts, such as logging or retry wrappers. Even then, pass them straight through so the inner function still rejects bad names.

</details>

<details>
<summary><b>And <code>*args</code>?</b></summary>

When the caller supplies any number of similar values, like `total(*numbers)`. For a known list of items, accepting one list argument is often clearer.

</details>

<details>
<summary><b>Why does Python require defaults after non-defaults?</b></summary>

Otherwise a call like `add(10)` would be ambiguous: does `10` fill the first or the second parameter? Python rejects the definition rather than guessing.

</details>

---

## 💬 Q6. A request counter works locally but gives strange numbers in production. The code uses a global variable. Why?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "A global lives in one process. In production there are several worker processes, so each has its own counter, and inside one process several threads can update it at once, so increments get lost. Locally, one process with one request at a time hides both problems. Shared counts belong in a shared store, such as a database or Redis with an atomic increment, or in the metrics system. Inside the code I'd pass state through arguments and return values instead of `global`, which also makes it testable."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why does <code>count += 1</code> inside a function raise <code>UnboundLocalError</code>?</b></summary>

Any assignment to a name inside a function makes that name **local** for the whole function, so reading it before the assignment fails. `global count` (or `nonlocal` for an enclosing function) changes that. Python looks names up in this order: Local → Enclosing → Global → Built-in (**LEGB**).

</details>

<details>
<summary><b>But <code>items.append(x)</code> on a global list works. Why?</b></summary>

`append` changes the object; it doesn't reassign the name, so no local is created. That's exactly why a shared mutable global can leak data between requests.

</details>

<details>
<summary><b>Are globals ever fine?</b></summary>

Constants, settings read once at start-up, and shared clients (an HTTP or database client that is safe across threads). Not anything that changes per request.

</details>

---

## 🏋️ Practical drill (15 minutes)

**Part A: dry run.** Without running it, write down every line of output:

```python
for i in range(0, 11):
    if i in (3, 4, 5):
        continue
    if i == 7:
        for j in range(0, 300):
            if j == 3:
                break
            print("welcome")
    if i == 9:
        break
    print(i)
else:
    print("no break")
print("done")
```

**Part B: predict.**

```python
print(bool("False"), bool([]), bool(0.0), bool(" "))
def greet(name, message="Hi"): return f"{message}, {name}"
print(greet("Asha"), greet(message="Yo", name="Ravi"))
def show(*args, **kwargs): return args, kwargs
print(show(1, 2, email="x@y.com"))
print(list(enumerate("ab", start=1)))
```

<details>
<summary>🔑 <b>Answers</b> (open after trying)</summary>

**Part A:** `0 1 2 6`, then `welcome` three times, then `7 8`, then `done` (each on its own line). `9` is never printed because `break` comes first, and `no break` never prints because the loop ended with `break`. The inner `break` stopped only the inner loop.

**Part B:**

| Line | Output | Why |
|:---:|---|---|
| 1 | `True False False True` | A non-empty string is true, even `"False"` or a space |
| 2 | `Hi, Asha Yo, Ravi` | Default used; keyword order doesn't matter |
| 3 | `((1, 2), {'email': 'x@y.com'})` | Tuple for extra positionals, dict for extra keywords |
| 4 | `[(1, 'a'), (2, 'b')]` | Counter starts at 1 |

</details>

## 🧾 Candidate evidence to prepare

- 🔁 Where your products retry or degrade gracefully → [Aura in the story bank](../../03-story-bank/README.md)
- 🛂 A human-approval gate you built (the Aura admin review)
- 🔐 Consent handling in Aaina (no photo analysed without consent)

> [!IMPORTANT]
> **✍️ To verify:**
> - the exact retry/circuit-breaker settings in your code before quoting numbers
> - one AI-generated code mistake you caught yourself

## 🔍 Open questions

- Exception handling, file handling and the `os` module are self-study. Come back and add a short page if the next class only skims them
- The OOP pillars start next class. Prepare a precise answer on Python encapsulation (see the accuracy note)

---

⬅️ [Session 03 — Collections, copying and strings](../2026-10-17-python-collections-strings/README.md) · 🏠 [README](../../README.md)

🔗 Related: [AI systems](../../02-question-banks/ai-systems.md) · [Behavioral](../../02-question-banks/behavioral.md) · [Story bank](../../03-story-bank/README.md)
