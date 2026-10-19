# 🔤 Session 03 — Python recap, part 2: collections, copying and strings

![Date](https://img.shields.io/badge/date-17%20Oct%202026-2563eb?style=flat-square)
![Type](https://img.shields.io/badge/type-Python%20recap%20%2B%20Q%26A-9333ea?style=flat-square)
![Questions](https://img.shields.io/badge/interview%20questions-5-16a34a?style=flat-square)
![Drill](https://img.shields.io/badge/drill-15%20min-f59e0b?style=flat-square)

> [!NOTE]
> **📼 Source:** supplied VTT transcript `GMT20261017-022902_Recording.cc.vtt`. Not stored in this repository.
>
> **🧪 What was taught:**
> - sets and dictionaries
> - type conversion
> - `input()` and `print()`
> - references, shallow copies and deep copies
> - strings in depth: indexing, slicing, operators, methods, split/join, f-strings, escape characters
>
> **Moved to the next class:** control flow, loops and functions.
>
> The interview questions are **independent preparation**. They turn each topic into the kind of bug or customer problem an FDE actually meets.

---

## 📚 What the instructor covered

### 🧺 Sets

- `set()` makes an empty set. **`{}` makes an empty dict**, a common trap.
- Use `add(x)` to add. Prefer `discard(x)` (silent if missing) over `remove(x)`, which raises `KeyError`.
- Set maths: union `A | B`, intersection `A & B`, difference `A - B` (in A but not in B).
- **FDE example from class:** two CCTV cameras each produce a set of detected names. `cam1 & cam2` gives the people seen in both rooms.

### 🗺️ Dictionaries

| Point | Detail |
|---|---|
| Keys | Unique and **hashable**. A repeated key keeps the **latest** value |
| Structure | One nested dict keyed by roll number beats several parallel lists. Think of a register |
| Safe read | `d.get(k)` returns `None` (or a default) instead of raising `KeyError` like `d[k]` |
| Views | `keys()`, `values()`, `items()` (pairs as tuples) |
| Change | `pop(k)`, `clear()`, `update(other)` merges, `d[101]["fees"] = 4000` edits a nested value |

Jupyter shows the last expression of a cell automatically; a `.py` file needs `print()`.

### 🔁 Type conversion ("typecasting")

| Call | Result |
|---|---|
| `float(12)` | `12.0` |
| `int("12")`, `float("12.5")` | ✅ works |
| `int("12.5")` | ❌ `ValueError`. Use `int(float("12.5"))` |
| `float("hello")` | ❌ `ValueError` |
| `list("12")` | `['1', '2']` |
| `list(12)` | ❌ an int is not iterable |
| `list(d.keys())`, `list(set_)`, `set(list_)` | ✅ `set(list_)` removes duplicates |

### ⌨️ Input and output

- `input("Enter a number: ")` shows a prompt and **always returns a string**. `"100" + "200"` is `"100200"`.
- Convert at the boundary: `float(input(...))`. The instructor preferred `float` because it accepts both `10` and `10.5`.

### 🔗 References and copying

| Code | What happens |
|---|---|
| `b = a` | **No copy.** Both names point to the same list. `b.append(3)` shows up in `a` |
| `a = [1, 2]` later | `a` now points to a new list; `b` still holds the old one |
| `b = a.copy()` | **Shallow copy:** a new outer list, but the **same** inner objects |
| `b = copy.deepcopy(a)` | **Deep copy:** new outer list **and** new nested objects |

The class demo: with `a = [[1, 2, 3], [4, 5, 6]]` and `b = a.copy()`, running `a[0].append(456)` also changes `b[0]`. The reason is that a list stores **references** to its items, not the items themselves.

### 🔤 Strings

| Topic | Key point |
|---|---|
| Quotes | `'...'` and `"..."` are the same. `"""..."""` allows several lines |
| Comments | `#` for one line. A bare `"""..."""` not assigned to anything is used as a multi-line comment |
| Indexing | From the left, `0 … len-1`. From the right, `-1 … -len`. `s[-1]` beats `s[len(s) - 1]` |
| Slicing | `s[start:end:step]`, **end is exclusive**. Omitted start/end mean "from the beginning"/"to the end". `s[::-1]` reverses |
| Operators | `"ab" * 3` repeats · `+` joins · `in` checks a substring · `==` compares |
| Case | `upper`, `lower`, `title`, `capitalize`, `swapcase` |
| Whitespace | `strip`, `lstrip`, `rstrip` |
| Search | `find` (index or **-1**), `count`, `startswith`, `endswith`, `replace(old, new)` |
| Split/join | `s.split()` returns a **list**. `"-".join(list_of_str)` glues it back; it fails if any item isn't a string |
| Checks | `isdigit`, `isalnum`, `islower`, `isupper` |
| f-strings | `f"{name} was born on {dob}"` · `f"{x:.2f}"` rounds **only for display** |
| Escapes | `\n` new line, `\t` tab |

Strings are immutable. `text.strip()` returns a **new** string, so save it: `text = text.strip()`.

**Email clean-up example from class:** `email.strip().lower()`, then `email.split("@")[-1]` for the provider. The instructor preferred `[-1]` over `[1]` because it never raises `IndexError`. (See the accuracy note below.)

**Assignments set:**
- research what a hash is
- optional: `str.format()`
- revise `if/elif/else`, `for`/`while` loops and function basics before the next class

### 🎙️ From the cohort Q&A (summarised, no participant details)

| | Point |
|:---:|---|
| 🧠 | Shallow vs deep copy, explained again with a memory diagram. A list is a **container of references**; a shallow copy makes a new container holding the same references |
| 🔧 | `a.copy()` is a list **method**; `copy.deepcopy(a)` is a function from the `copy` **module** |
| 🚢 | **CI/CD:** GitHub Actions is now the common default; Jenkins needs more server setup |
| ☸️ | **Kubernetes is not in the syllabus**: it takes months to learn and is costly to run. Containers, cloud basics and CI/CD are covered. Docker is packaging, not orchestration. Many company AI projects stall at the proof-of-concept (POC) stage, so learn to build and ship first |
| 🗣️ | **Soft skills:** fluent, confident English; ask questions that get the task done. The projects later in the course cover client discovery |
| 🧾 | **"Detect fraud documents with AI" is harder than it sounds.** Reasoning over text is one thing; detecting tampering may need computer vision or ML. Sending every image to a paid API is easy but costly. Rules (signed or not, right kind of photo) can go first |
| 🧭 | **FDE vs specialist:** an FDE uses RAG and agents to solve customer problems; a specialist goes deeper into one area |

---

## 🎯 Accuracy notes (independent, checked against Python behaviour)

> [!WARNING]
> The live recap simplified a few points. **Use the precise version in interviews:**
>
> - 📧 **`split("@")[-1]` hides bad input.** For `"alex"` with no `@` it returns `"alex"` as the "provider" without raising an error. That is a silent data bug. Check first: `local, sep, domain = email.partition("@")`, and reject the input if `sep` is empty.
> - 🔢 **`isdigit()` is not a number validator.** `"-5"`, `"1.5"` and `" 7"` all return `False`. Use `try: float(s) except ValueError:` and then check the range.
> - 🔤 **`isalnum()` is `False` if there is any space or punctuation**, so `"Batch 1"` returns `False` even though it has letters and digits.
> - 🧱 **`split()` and `split(" ")` differ.** With no argument, `split()` treats any run of whitespace as one separator and drops empty items: `"ABC   DEF".split()` gives `['ABC', 'DEF']`, but `split(" ")` gives empty strings. Use `len(text.split())` for a word count.
> - 💬 **A bare triple-quoted string is not a real comment.** It's a string expression that Python evaluates and throws away. Placed first in a module, class or function it becomes the **docstring**. Use `#` for comments.
> - 🧮 **Small integers are cached in CPython.** The class diagram showed a "new" `3` stored elsewhere. In CPython, `-5` to `256` are shared objects. That's a detail of one implementation, which is why you compare with `==`.
> - 💰 **`float(input())` is fine for a demo, not for money or counts.** Use `int` for counts, `Decimal` for money, and catch `ValueError` for bad input (see [Session 02](../2026-10-11-python-fundamentals/README.md)).
> - 🔍 **`find` returns -1, `index` raises.** `-1` is also a valid index, so `s[s.find(x)]` silently reads the last character when `x` is missing.

## 🔑 Assignment answer: what is a hash?

> [!NOTE]
> Independent answer to the class assignment.

- `hash(obj)` turns an object into an integer. Dicts and sets use that integer to find a **bucket** directly instead of scanning, so lookups are **O(1) on average**.
- **The rule:** if `a == b` then `hash(a) == hash(b)`. That's why keys must not change while stored. Immutable values (str, int, tuple of immutables) are hashable; `list`, `dict` and `set` are not.
- Two different keys can share a hash (a **collision**). The table then also compares with `==`, so the result is still correct, just slower.
- `str` hashes are **randomised per process** (`PYTHONHASHSEED`), so never store `hash()` output or use it as an ID. For a stable fingerprint use `hashlib.sha256`.

## ⏱️ 10-minute recap

Most customer data arrives as **text**: form fields, CSV rows, API payloads, model output. Three habits prevent most bugs:

| Habit | Example |
|---|---|
| 🧼 **Normalise at the boundary, once** | `email = email.strip().lower()` when the request arrives |
| ✅ **Convert and validate explicitly** | `int(row["qty"])` inside `try/except ValueError`, then a range check |
| 🔗 **Know when you share and when you copy** | `b = a` shares; `copy.deepcopy` for nested data that will change |

Pick the structure by the question you need answered: "seen before?" → `set`; "look up by key" → `dict`; "who is in both?" → `&`.

## ⚖️ Key decisions and trade-offs

| Need | Choose | ⚠️ Watch out |
|---|---|---|
| Read a key that may be missing | `d.get(k, default)` | Hides a typo in the key name. Use `d[k]` when missing means a bug |
| Independent copy of nested config | `copy.deepcopy` | Slower and uses more memory on big objects |
| Copy a flat list | `a.copy()` / `a[:]` / `list(a)` | Nested items are still shared |
| Word count | `len(text.split())` | Words ≠ LLM tokens; meter tokens for model limits |
| Parse a number from text | `int()` / `float()` / `Decimal()` + `try` | `float` for money drifts |
| Extract an email domain | `partition("@")` + check | `split("@")[-1]` accepts junk silently |

---

## 💬 Q1. Some users can't log in, and support suspects their email addresses. How would you investigate and fix it?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "First I'd get two or three failing accounts and compare what the user types with what is stored. The usual causes are spaces from copy-paste, different capital letters, and duplicate accounts created because of them. The fix is to normalise once, at the boundary: strip, then lower-case, then validate the shape with `partition('@')` so missing parts fail loudly. I'd make the database enforce uniqueness on the normalised value. Then I'd write a one-off check that finds existing accounts that now collide, because those need a human decision rather than an automatic merge."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why not just take <code>split("@")[-1]</code> for the domain?</b></summary>

It never raises, so bad input passes silently: `"alex".split("@")[-1]` is `"alex"`. I'd rather reject the address at sign-up with a clear message than store junk and find it in a report months later.

</details>

<details>
<summary><b>Is lower-casing the whole address correct?</b></summary>

Strictly, the part before `@` can be case-sensitive, but almost every real provider treats it as case-insensitive, and duplicate accounts are the bigger risk. I'd store the normalised version for matching and keep the original for display. If a customer's provider really is case-sensitive, that's a requirement to confirm with them.

</details>

<details>
<summary><b>Two existing accounts collide after normalising. What do you do?</b></summary>

Don't auto-merge, because that can hand one person's data to another. List the collisions, check which account was used recently, and let support confirm with the user. Block new duplicates first so the list stops growing.

</details>

<details>
<summary><b>Where exactly does normalisation live?</b></summary>

In one function at the input boundary (API schema or sign-up handler), used by sign-up, login and password reset alike. If each screen does its own version, they drift apart.

</details>

---

## 💬 Q2. Each new customer tenant starts from a default config template. One tenant's change appeared in another tenant's settings. What happened?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "Almost certainly the template was shared instead of copied. Either `config = DEFAULT` or `DEFAULT.copy()`. A shallow copy gives a new outer dict, but the nested lists and dicts inside are still the same objects, so editing a nested value changes the template for everyone. I'd reproduce it with two tenants in a test, then fix it by building each tenant's config with `copy.deepcopy`, or better, from a function that returns a fresh dict each time. Then I'd check whether any tenant data already leaked, because in a multi-tenant product that's a privacy incident, not just a bug."

> [!IMPORTANT]
> **✍️ Rav:** if one of your multi-tenant products (Aaina, Poultry 360) had per-tenant settings, say how you create them. Only claim a bug you really had.

**❓ Follow-ups** · *click to open*

<details>
<summary><b>When is a shallow copy enough?</b></summary>

When the items are immutable (numbers, strings, tuples of those), or when you only change the top level. For a flat list of IDs, `a.copy()` is fine.

</details>

<details>
<summary><b>Why might a factory function be better than <code>deepcopy</code>?</b></summary>

`def default_config(): return {...}` builds new objects every call. It's explicit, fast, and can't accidentally share. `deepcopy` copies whatever is there, including objects you didn't mean to copy (clients, locks), and is slower on large data.

</details>

<details>
<summary><b>How would you stop this happening again?</b></summary>

- a test that edits tenant A and asserts tenant B is unchanged
- read-only templates where possible (`types.MappingProxyType`, frozen dataclasses)
- code review for `= DEFAULT`-style assignments

</details>

<details>
<summary><b>How do you tell the customer?</b></summary>

Facts first: what was exposed, to whom, since when, and what is fixed. Then what you are checking next. Follow the customer's incident process, and don't guess at the impact before the logs confirm it.

</details>

---

## 💬 Q3. Product wants "free users can send prompts up to 50 words." How do you build the limit?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "First I'd ask what the limit is protecting. If it's model cost, words are the wrong unit. The model bills in tokens, and the token count per word varies a lot by language, so I'd count tokens with the model's tokenizer or use the provider's token counting. If it really is a product rule about words, `len(text.split())` handles repeated spaces correctly, while counting the spaces doesn't. Either way the check runs on the server, because a browser-side check is only for user experience, and the error message tells the user the limit and their current count."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why not count spaces + 1?</b></summary>

`"ABC   DEF".count(" ")` is 3, which suggests 4 words when there are 2. Leading or trailing spaces and new lines break it too. `split()` with no argument handles all of these.

</details>

<details>
<summary><b>What about Hindi or mixed-script text?</b></summary>

`split()` splits on whitespace, so it works for Hindi written with spaces, but scripts without spaces (Chinese, Japanese) need a proper word segmenter. And tokens per word are usually higher for Indian languages than for English, which is another reason to meter cost in tokens.

</details>

<details>
<summary><b>Where else should limits live?</b></summary>

Also cap the **output** tokens and the number of requests per user per minute. A short prompt can still ask for a very long answer.

</details>

---

## 💬 Q4. A customer sends a CSV where the quantity column has values like `12`, `12.5`, `""` and `1,200`. How do you load it safely?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I'd never let `float()` on every value be the whole plan. First I agree the rules with the customer: is `12.5` a valid quantity, does `1,200` mean twelve hundred, what should a blank mean? Then the parser follows those rules: strip, remove thousand separators if agreed, convert inside `try/except ValueError`, check the range, and use `int` for counts or `Decimal` for money. Rows that fail go to a reject file with the row number and reason, and I report the counts, loaded and rejected, back to the customer. Silent guessing is the worst outcome, because the numbers look fine and are wrong."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why not just use <code>isdigit()</code> to check?</b></summary>

It returns `False` for `"-5"`, `"1.5"` and `" 7"`, and `True` for some Unicode digits that `int()` may not treat the way you expect. Trying the conversion is the honest test.

</details>

<details>
<summary><b>Why not convert everything with <code>float</code>, like the class demo?</b></summary>

A quantity of `12.5` boxes may be invalid, and `float` silently accepts it. Floats also drift for money. The type should express the business rule.

</details>

<details>
<summary><b>The file has 2 million rows. Does the approach change?</b></summary>

Stream it with `csv.reader` (or pandas in chunks) rather than loading everything, and still write rejects with their row numbers. The validation rules stay the same.

</details>

<details>
<summary><b>What if 30% of rows fail?</b></summary>

Stop and talk to the customer before loading anything. That rate usually means the rules are wrong (a different export format, a locale using `,` as the decimal mark), not that the data is bad.

</details>

---

## 💬 Q5. A bank asks you to "use AI to detect fraudulent documents." What do you do in the first week?

> [!TIP]
> **🗣️ Say this (≈75 s)**
>
> "I'd start by making 'fraud' concrete. Which documents, which kinds of fraud (edited numbers, fake stamps, wrong photo, mismatched details), how many a day, and how reviewers catch them today. Then I'd ask what a miss costs versus a false alarm, and what data I'm allowed to use. Sending ID images to an outside API may not be acceptable at all. Often the first useful version is cheap: rule checks such as a missing signature, mismatched names across documents, or an unexpected file type, ranking documents so a human reviews the riskiest first. Only if a measured gap remains would I add vision models or an LLM, and I'd show the trade-off between cost and detection rate on their own past cases."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why not send every document to a vision LLM?</b></summary>

- cost grows with every page
- possible privacy or regulatory limits on sending documents out
- a model that "reasons" about a document can't reliably detect pixel-level tampering

It may still be useful for pulling out fields to cross-check. That's a different job from forensic detection.

</details>

<details>
<summary><b>How would you measure success?</b></summary>

Use a labelled set of their past cases, with genuine and confirmed fraud kept separate. Report **recall** (frauds caught) and **precision** (alerts that were real), and also reviewer time saved. Agree the target with the customer before building.

</details>

<details>
<summary><b>They want "100% detection." How do you respond?</b></summary>

Be honest: no system promises that, and chasing it floods reviewers with false alarms. Offer a measured rate on their data, a human review step for flagged and borderline cases, and a way to feed confirmed misses back into the system.

</details>

<details>
<summary><b>Where does the solution run?</b></summary>

Wherever their policy requires, often inside their own network. That limits which models you can use, so ask on day one.

</details>

---

## 🏋️ Practical drill (15 minutes)

Predict each output **before** running it:

```python
print(type({}), type(set()))
a = [[1, 2], [3]]; b = a.copy(); a[0].append(9); print(b)
import copy; c = copy.deepcopy(a); a[1].append(7); print(c)
s = "FDE Bootcamp"
print(s[0:3], s[-1], s[::-1][:4], s[0:4:-1])
print("ABC   DEF".split(), "ABC   DEF".split(" "))
print("alex".split("@")[-1], "x".find("z"), "-5".isdigit(), "Batch 1".isalnum())
print({"a": 1, "a": 2}, int(float("12.5")))
print(f"{2/3:.2f}", "ab" * 3, "ll" in "hello")
```

✍️ For each wrong prediction, write one sentence on **how it would show up as a customer bug**.

<details>
<summary>🔑 <b>Answers</b> (open after trying)</summary>

| Line | Output | Why |
|:---:|---|---|
| 1 | `<class 'dict'> <class 'set'>` | `{}` is an empty dict |
| 2 | `[[1, 2, 9], [3]]` | Shallow copy shares the inner list |
| 3 | `[[1, 2, 9], [3]]` | Deep copy taken before `append(7)`, so it is independent |
| 4 | `FDE p pmac ` (an empty string at the end) | End is exclusive; a negative step from 0 has nowhere to go |
| 5 | `['ABC', 'DEF'] ['ABC', '', '', 'DEF']` | `split()` merges whitespace runs; `split(" ")` doesn't |
| 6 | `alex -1 False False` | Silent junk domain; `-1` means not found; `-` isn't a digit; the space fails `isalnum` |
| 7 | `{'a': 2} 12` | Latest key wins; `int` truncates `12.5` |
| 8 | `0.67 ababab True` | Display rounding; repetition; substring check |

</details>

## 🧾 Candidate evidence to prepare

- 🧼 One place in your own code where you **normalise or validate input at the boundary** (for example a Pydantic schema in a FastAPI service)
- 🏢 How per-tenant settings are created in your multi-tenant products → [story bank](../../03-story-bank/README.md)

> [!IMPORTANT]
> **✍️ To verify:**
> - whether any of your products meter usage by tokens, and how
> - a real input-parsing or copy bug you fixed (open-source PRs often have one)

## 🔍 Open questions

- `str.format()` and the `match` statement were left as self-study
- How the course will handle containers and cloud deployment without Kubernetes, and what "production ready" means for course projects

---

⬅️ [Session 02 — Python fundamentals, part 1](../2026-10-11-python-fundamentals/README.md) · ➡️ [Session 04 — Control flow and functions](../2026-10-18-python-control-flow-functions/README.md) · 🏠 [README](../../README.md)

🔗 Related: [Customer discovery](../../02-question-banks/customer-discovery.md) · [Story bank](../../03-story-bank/README.md)
