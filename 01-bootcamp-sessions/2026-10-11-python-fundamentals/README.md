# 🐍 Session 02 — Python fundamentals recap, part 1

![Date](https://img.shields.io/badge/date-11%20Oct%202026-2563eb?style=flat-square)
![Type](https://img.shields.io/badge/type-Python%20recap%20%2B%20Q%26A-9333ea?style=flat-square)
![Questions](https://img.shields.io/badge/interview%20questions-4-16a34a?style=flat-square)
![Drill](https://img.shields.io/badge/drill-10%20min-f59e0b?style=flat-square)

> [!NOTE]
> **📼 Source:** supplied VTT transcript `GMT20261011-022742_Recording.cutfile.20261011101008198.transcript.vtt`. Not stored in this repository.
>
> **🧪 What was taught:** variables and object references, data types, mutable vs immutable objects, operators, and the first collections (list, tuple, set). **Postponed to the next class:** dictionaries, strings, control flow, loops, functions and membership operators.
>
> The interview questions are **independent preparation**. The instructor said directly that FDE interviews are unlikely to quiz basic Python.

---

## 📚 What the instructor covered

### 🏷️ Variables

- A variable is **a name that refers to an object**. `a = 3` creates an int object in memory, and `a` holds a reference to it. `id(a)` shows a number that stands for that reference.
- **Naming rules:**
  - letters, digits and `_` only
  - must not start with a digit; no spaces
  - use meaningful **`snake_case`** names (`first_name`, not `a` or `FirstName`)
- Assigning a new value to the same name **replaces the reference**.

### 🧱 Data types

| Group | Types | Notes |
|---|---|---|
| 🔢 Numbers | `int`, `float`, `complex` | Python infers the type; no declarations |
| ✅ Boolean | `bool` | `True` / `False`, capitalised |
| 🔤 Text | `str` | Single or double quotes mean the same. `'123'` and `'True'` are strings |
| 📋 Sequences | `list`, `tuple`, `range` | |
| 🧺 Set | `set` | |
| 🗺️ Mapping | `dict` | |
| ∅ Nothing | `None` (`NoneType`) | |

### 🔒 Mutable vs immutable (shown with `id()`)

| 🔒 Immutable: a change creates a **new** object | 🔓 Mutable: changes in place, **same** object |
|---|---|
| `int`, `float`, `bool`, `str`, `tuple` | `list`, `dict`, `set` |
| `a = a + 1` → new `id` | `my_list.append(x)` → same `id` |

Objects nothing refers to any more are cleaned up by **garbage collection**. "Primitive vs non-primitive" is a Java/C++ idea that Python doesn't really have, and the instructor said FDE interviews won't ask it.

### ➗ Operators

| Operator | Meaning | Example → result |
|:---:|---|---|
| `/` | True division | `10 / 3` → `3.33…` |
| `//` | Floor division | `10 // 3` → `3` |
| `%` | Remainder | `10 % 3` → `1` |
| `**` | Power | `2 ** 3` → `8` |
| `==` `!=` `>` `<` | Comparison | always returns a `bool` |
| `and` `or` `not` | Logical | combine or flip booleans |
| `is` | Identity | same object? |

Bitwise operators were shown briefly. **Operator precedence** was set as a small exercise.

### 📦 First collections

| | 📋 List | 📌 Tuple | 🧺 Set |
|---|---|---|---|
| **Syntax** | `[1, 2]` | `(1, 2)` · one item: `(1,)` | `{1, 2}` or `set()` |
| **Ordered?** | ✅ | ✅ | ❌ |
| **Changeable?** | ✅ | ❌ | ✅ |
| **Duplicates?** | ✅ | ✅ | ❌ removed |
| **Key methods** | `append`, `remove(x)`, indexing from 0 | read and index only | `add` (no `append`), union, intersection |
| **Use for** | Ordered items you add to | Values that must not change | De-duplication, membership |

### 🎙️ From the cohort Q&A (summarised, no participant details)

| | Point |
|:---:|---|
| 🧭 | **FDE right now** = someone who can integrate AI solutions into a company's systems. What's expected varies by company, so read each job description |
| 🧑‍💼 | **Experienced engineers:** map existing skills to the job description. Most will already match; the course fills the AI part |
| 🧪 | **QA backgrounds:** testing agents and MCP (Model Context Protocol) servers is growing work |
| ⚠️ | **Interviews probe the parts you skipped**, so don't skip topics |

---

## 🎯 Accuracy notes (independent, checked against Python behaviour)

> [!WARNING]
> The live recap simplified a few points. **Use the precise version in interviews:**
>
> - 🔢 **Python `int` has arbitrary precision.** Huge integers work without NumPy. `float` is 64-bit IEEE-754, so `0.1 + 0.2 != 0.3`. **For money use `decimal.Decimal`**, not `float` and not NumPy.
> - 🆔 **`is` checks identity, `==` checks value.** `a + 0` keeping the same `id` is CPython reusing small integer objects, an implementation detail. **Use `is` only for `None`.**
> - 🗺️ **`dict` keeps insertion order** (guaranteed since Python 3.7). `set` does not.
> - 🔒 **Immutability is shallow.** A tuple holding a list can't be reassigned, but the list inside can still change.

## ⏱️ 10-minute recap

In Python, **names point to objects**. Some objects change in place (list, dict, set); some can't (int, float, str, tuple, bool). This one idea explains real bugs:

- 👯 two names silently sharing one list
- 🧟 a default argument that keeps state between calls
- 💸 a float rounding error in a bill

**Pick the collection by the question you need answered fast:**

| Question | Pick |
|---|---|
| Does order matter? | 📋 `list` |
| Is it a fixed record? | 📌 `tuple` |
| "Have I seen this?" | 🧺 `set` |
| Look up by key | 🗺️ `dict` |

## ⚖️ Key decisions and trade-offs

| Need | Choose | ⚠️ Watch out |
|---|---|---|
| Ordered items you add to | `list` | `x in list` scans everything, so it's slow for big lookups |
| Fixed record, safe to share or use as a dict key | `tuple` | Only shallowly immutable |
| De-duplicate IDs, fast membership | `set` | No order; items must be hashable |
| Look up by key (user id → record) | `dict` | Keys must be hashable |
| Money or exact decimals | `Decimal` | `float` rounding errors add up |
| "No value" check | `x is None` | `if not x` also treats `0`, `""`, `[]` as missing |

---

## 💬 Q1. Python isn't your main language. How would you show you can write production Python?

> [!TIP]
> **🗣️ Say this (personalise first, ≈60 s)**
>
> "Most of my professional years were in Java. For the last part I have built and run Python services in production: FastAPI backends for my live products, with tests and CI. I have also had pull requests merged in Python open-source libraries such as pypdf, where maintainers review every line for edge cases, tests and style. That review taught me to read a codebase's conventions first and to cover edge cases with tests before I push."

> [!IMPORTANT]
> **✍️ Rav:** confirm your exact Java vs Python years, and which PRs you can walk through without notes.

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Show one Python bug you fixed.</b></summary>

Pick one merged pypdf PR you can explain fully: **reproduction → cause → fix → test**.

</details>

<details>
<summary><b>What's different coming from Java?</b></summary>

| Java | Python |
|---|---|
| Static types | Dynamic typing → **add type hints**, validate data at boundaries (e.g. Pydantic) |
| Interfaces | Duck typing |
| Real parallel threads | **GIL** limits CPU-heavy threads → use processes or async for I/O |

</details>

<details>
<summary><b>How do you keep Python code safe without a compiler?</b></summary>

Type hints + a type checker · tests · linting in CI · validation of all external input.

</details>

---

## 💬 Q2. A customer's report shows a total that's off by a few paise. What could cause it?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "My first suspect is float arithmetic. Floats can't store most decimal fractions exactly, so sums drift. I'd reproduce the calculation on the real input, then switch money to `Decimal` with an explicit rounding rule agreed with the customer, or store amounts as integer paise. I'd add a test with the exact failing case, and check where the data comes from. If the database or an API already returns floats, the fix has to go there too."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why not just round at the end?</b></summary>

Rounding at the end hides the error in totals but not in line items, and different rounding rules give different answers. **The business must choose the rule.**

</details>

<details>
<summary><b>Integer paise or <code>Decimal</code>?</b></summary>

| Option | Best when |
|---|---|
| Integer paise | One currency; simple and fast |
| `Decimal` | Mixed precision or tax rules; clearer |

</details>

---

## 💬 Q3. Explain mutable vs immutable with a bug you might see in real code.

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "Lists and dicts change in place, so two names can point to the same list, and a change through one shows up in the other. A classic bug is a mutable default argument: `def add(item, items=[])` reuses the same list on every call, so data leaks between calls. In a web service that can mean one request seeing another request's data. The fix is `items=None` and creating a new list inside. Immutable values like tuples and strings are safer to share."

```python
# ❌ bug: the same list is reused on every call
def add(item, items=[]):
    items.append(item)
    return items

# ✅ fix
def add(item, items=None):
    items = [] if items is None else items
    items.append(item)
    return items
```

**❓ Follow-ups** · *click to open*

<details>
<summary><b>How would you find this in a large codebase?</b></summary>

Linters flag mutable defaults, and a test that calls the function **twice** catches it.

</details>

<details>
<summary><b>Shallow vs deep copy?</b></summary>

`copy()` copies the outer container only; `deepcopy()` copies nested objects too. Choose based on whether the nested data will change.

</details>

<details>
<summary><b>Why does <code>is</code> sometimes work for numbers?</b></summary>

CPython caches small integers. It's an implementation detail, so **use `==`**.

</details>

---

## 💬 Q4. 2 million event IDs: drop duplicates and check membership fast. Which data structure?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "A set. Membership in a set is constant time on average; in a list it's a full scan. If I must keep the first-seen order, I'd use `dict.fromkeys(ids)` as an ordered set, or a set for checking plus a list for output. If the data doesn't fit in memory, I'd de-duplicate in the database or stream it in chunks."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>What's the cost?</b></summary>

Sets use more memory than lists. For huge data, consider the database, a sorted merge, or a probabilistic filter if a small error rate is acceptable.

</details>

<details>
<summary><b>What must the items be?</b></summary>

**Hashable:** strings, numbers and tuples of those. Not lists.

</details>

---

## 🏋️ Practical drill (10 minutes)

Predict each output **before** running it in a Python shell:

```python
a = [1, 2]; b = a; b.append(3); print(a)
t = (1, [2]); t[1].append(3); print(t)
print(0.1 + 0.2 == 0.3)
print(10 / 3, 10 // 3, 10 % 3, 2 ** 3)
print(set([3, 1, 3, 2]))
def f(x, acc=[]): acc.append(x); return acc
print(f(1), f(2))
```

✍️ For each wrong prediction, write one sentence on **why**, and how it could show up as a customer bug.

<details>
<summary>🔑 <b>Answers</b> (open after trying)</summary>

| Line | Output | Why |
|:---:|---|---|
| 1 | `[1, 2, 3]` | `b` and `a` are the same list |
| 2 | `(1, [2, 3])` | Tuple is immutable, but the list inside isn't |
| 3 | `False` | Float rounding |
| 4 | `3.3333333333333335 3 1 8` | True div, floor div, remainder, power |
| 5 | `{1, 2, 3}` | Duplicates removed; don't rely on the order |
| 6 | `[1, 2] [1, 2]` | Mutable default shared, and both calls return the same list |

</details>

## 🧾 Candidate evidence to prepare

- 🔀 One merged Python PR (e.g. in pypdf) you can explain end to end → [story 6 in the story bank](../../03-story-bank/README.md)

> [!IMPORTANT]
> **✍️ To verify:**
> - your honest years of Python in production versus Java
> - one real Python bug from your own products

## 🔍 Open questions

- Next class: dictionaries, strings, control flow, loops, functions and membership operators → add as **part 2**
- Type hints, testing and packaging were not covered. Prepare them independently; interviewers probe them more than syntax

---

⬅️ [Session 01 — Developer environment](../2026-10-10-dev-environment-uv/README.md) · 🏠 [README](../../README.md)

🔗 Related: [AI systems](../../02-question-banks/ai-systems.md) · [Story bank](../../03-story-bank/README.md)
