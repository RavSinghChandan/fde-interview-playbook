# Session 02 — Python fundamentals recap, part 1 (11 October 2026)

**Source:** supplied VTT transcript, `GMT20261011-022742_Recording.cutfile.20261011101008198.transcript.vtt`. The source file is not stored in this repository.
**Session type:** fast Python recap for a mixed cohort, followed by a long Q&A on careers and course choice.
**What was actually taught:** variables and object references, data types, mutable vs immutable objects, operators, and the first collections (list, tuple, set). Dictionaries, strings, control flow, loops, functions and membership operators were postponed to the next class. The interview questions below are **independent preparation**. The instructor said directly that FDE interviews are unlikely to quiz basic Python.

## What the instructor covered

- **A variable is a name that refers to an object.** `a = 3` creates an int object in memory, and `a` holds a reference to it. `id(a)` shows a number that stands for that reference.
- **Naming rules:**
  - Names may contain letters, digits and underscores, but must not start with a digit or contain spaces.
  - Use meaningful names in `snake_case` (`first_name`, not `a` or `FirstName`).
  - Assigning a new value to the same name replaces the reference, so earlier values are no longer reachable through that name.
- **Data types:** numbers (`int`, `float`, `complex`), `bool` (`True`/`False`, capitalized), `str`, sequences (`list`, `tuple`, `range`), `set`, mapping (`dict`) and `None` (type `NoneType`). `type(x)` shows the type. Python infers types; you do not declare them.
- **Strings:** single or double quotes mean the same thing. Anything inside quotes is a string, even `'123'` or `'True'`. An empty string is still a string.
- **Mutable vs immutable**, demonstrated with `id()`:
  - `a = a + 1` on an int gives a **new** object (new id), so `int`, `float`, `bool`, `str` and `tuple` are immutable.
  - `list.append` keeps the **same** id, so `list`, `dict` and `set` are mutable.
  - Objects that nothing refers to any more are cleaned up by garbage collection.
- **"Primitive vs non-primitive"** is a Java/C++ idea. Python does not really have it, and the instructor said FDE interviews will not ask about it.
- **Operators:**
  - Arithmetic: `+ - * / // % **`. `/` is true division (`10/3` gives `3.33…`), `//` is floor division (`3`), `%` gives the remainder (`1`), `**` is power.
  - Comparison operators always return a bool.
  - Also covered: assignment, logical (`and`, `or`, `not`), identity (`is`) and a short look at bitwise operators. Operator precedence was set as a small exercise.
- **List:**
  - Ordered and mutable, created with square brackets.
  - `append` adds an item and `remove(x)` removes the first matching value.
  - Indexing starts at 0.
- **Tuple:**
  - Ordered and immutable, created with parentheses and commas. A one-item tuple needs a trailing comma: `(1,)`.
  - Use it for values that must not change.
- **Set:**
  - Curly braces or `set()`. Duplicates are removed and order is not guaranteed.
  - Use `add` (there is no `append`). Useful for union, intersection and membership checks.

### From the cohort Q&A (summarized, no participant details)

- **What FDE means right now:** someone who can integrate AI solutions into a company's systems. What kind of AI engineer is expected varies by company, so read each job description.
- **For experienced engineers:** map your existing skills to the job description. The instructor's view was that most of an experienced engineer's skills already match, and the course fills the AI part.
- **QA and testing backgrounds:** testing agents and MCP (Model Context Protocol) servers was mentioned as growing work.
- **Interviews probe the parts you skipped**, so do not skip topics while preparing.

## Accuracy notes (independent, checked against Python behaviour)

The live recap simplified a few points. Use the precise version in interviews:

- **Python `int` has arbitrary precision.** Very large integers work without NumPy. `float` is a 64-bit IEEE-754 number, so `0.1 + 0.2 != 0.3`. For money, use `decimal.Decimal`, not `float` and not NumPy.
- **`is` checks identity and `==` checks value.** `a + 0` showing the same `id` as `a` happens because CPython reuses small integer objects. That is an implementation detail, so never use `is` to compare numbers or strings. Use `is` only for `None` (`x is None`).
- **`dict` keeps insertion order** (guaranteed since Python 3.7). `set` does not.
- **Immutability is shallow.** A tuple containing a list cannot be reassigned, but the list inside it can still change.

## 10-minute recap

In Python, names point to objects. Some objects can change in place (list, dict, set) and some cannot (int, float, str, tuple, bool). This one idea explains several real bugs:

- Two names that silently share one list.
- A default argument that keeps state between calls.
- A float rounding error in a bill.

Pick the collection by the question you need to answer quickly:

- **list:** order matters.
- **tuple:** a fixed record.
- **set:** "have I seen this?"
- **dict:** look up by key.

## Key decisions and trade-offs

| Need | Choose | Why / watch out |
|---|---|---|
| Ordered items you will add to | `list` | `x in list` scans every item, so it is slow for large lookups |
| Fixed record, safe to share or use as a dict key | `tuple` | Only shallowly immutable |
| De-duplicate IDs, fast membership | `set` | No order; items must be hashable |
| Look up by key (user id → record) | `dict` | Keys must be hashable |
| Money or exact decimals | `Decimal` | `float` rounding errors add up |
| Check for "no value" | `x is None` | `if not x` also treats `0`, `""` and `[]` as missing |

## Interview question 1: Python is not your main language. How would you show you can write production Python?

**Ready answer (personalize first):** "Most of my professional years were in Java. For the last part I have built and run Python services in production: FastAPI backends for my live products, with tests and CI. I have also had pull requests merged in Python open-source libraries such as pypdf, where maintainers review every line for edge cases, tests and style. That review taught me to read a codebase's conventions first and to cover edge cases with tests before I push."

**Follow-ups:**

- *Show one Python bug you fixed.* Pick one merged pypdf PR you can explain fully: the reproduction, the cause, the fix and the test.
- *What is different coming from Java?* Python has dynamic typing, so I add type hints and validate data at boundaries (for example with Pydantic). It has duck typing instead of interfaces, and the GIL matters for CPU-heavy threads.
- *How do you keep Python code safe without a compiler?* Type hints with a checker, tests, linting in CI, and validation of all external input.

*Rav: confirm your exact Java vs Python years and which PRs you can walk through without notes.*

## Interview question 2: A customer's report shows a total that is off by a few paise. What could cause it?

**Ready answer:** "My first suspect is float arithmetic. Floats cannot store most decimal fractions exactly, so sums drift. I would reproduce the calculation on the real input, then switch money values to `Decimal` with an explicit rounding rule agreed with the customer, or store amounts as integer paise. I would add a test with the exact case that failed, and check where the data comes from. If the database or an API already returns floats, the fix has to go there too."

**Follow-ups:**

- *Why not just round at the end?* Rounding at the end hides the error in totals but not in line items, and different rounding rules give different answers. The business must choose the rule.
- *Integer paise or `Decimal`?* Integer paise is simple and fast for one currency. `Decimal` is clearer with mixed precision or tax rules.

## Interview question 3: Explain mutable vs immutable with a bug you might see in real code.

**Ready answer:** "Lists and dicts can change in place, so two names can point to the same list and a change through one shows up in the other. A classic bug is a mutable default argument: `def add(item, items=[])` reuses the same list on every call, so data leaks between calls. In a web service that can mean one request seeing another request's data. The fix is `items=None`, then create a new list inside the function. Immutable values such as tuples and strings are safer to share."

**Follow-ups:**

- *How would you find this in a large codebase?* Linters flag mutable defaults, and a test that calls the function twice catches it.
- *Shallow vs deep copy?* `copy()` copies the outer container only, while `deepcopy()` copies nested objects too. Choose based on whether the nested data will change.
- *Why does `is` sometimes work for numbers?* CPython caches small integers. That is an implementation detail, so use `==`.

## Interview question 4: You get 2 million event IDs and must drop duplicates and check membership quickly. Which data structure?

**Ready answer:** "A set. Checking membership in a set is constant time on average, while in a list it is a full scan. If I must keep the first-seen order, I would use a dict as an ordered set (`dict.fromkeys(ids)`) or keep a set for checking plus a list for output. If the data does not fit in memory, I would de-duplicate in the database or stream it in chunks."

**Follow-ups:**

- *What is the cost?* Sets use more memory than lists. For huge data, consider the database, a sorted merge, or a probabilistic filter if a small error rate is acceptable.
- *What must the items be?* Hashable: strings, numbers and tuples of those, not lists.

## Practical drill (10 minutes)

Answer in a Python shell, predicting the output **before** you run each line:

```python
a = [1, 2]; b = a; b.append(3); print(a)
t = (1, [2]); t[1].append(3); print(t)
print(0.1 + 0.2 == 0.3)
print(10 / 3, 10 // 3, 10 % 3, 2 ** 3)
print(set([3, 1, 3, 2]))
def f(x, acc=[]): acc.append(x); return acc
print(f(1), f(2))
```

For each line you predicted wrong, write one sentence on why and how it could appear as a customer bug.

## Candidate evidence to prepare

- One merged Python PR (for example in pypdf) that you can explain end to end. See [story 6 in the story bank](../../03-story-bank/README.md).
- *To verify:* your honest years of Python in production versus Java, and one real Python bug from your own products.

## Open questions

- The next class continues with dictionaries, strings, control flow, loops, functions and membership operators. Add them as part 2.
- The class did not cover type hints, testing or packaging in Python. Prepare them independently, since interviewers probe them more than syntax.

Related: [Session 01 — developer environment](../2026-10-10-dev-environment-uv/README.md) · [AI systems and production judgment](../../02-question-banks/ai-systems.md)
