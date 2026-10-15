# 🛠️ Session 01 — Developer environment: CLI, VS Code, uv, notebooks

![Date](https://img.shields.io/badge/date-10%20Oct%202026-2563eb?style=flat-square)
![Type](https://img.shields.io/badge/type-technical%20class%20%2B%20Q%26A-9333ea?style=flat-square)
![Questions](https://img.shields.io/badge/interview%20questions-5-16a34a?style=flat-square)
![Drill](https://img.shields.io/badge/drill-15%20min-f59e0b?style=flat-square)

> [!NOTE]
> **📼 Source:** supplied VTT transcript `GMT20261010-022752_Recording.cutfile.20261010104927990.transcript.vtt`, plus the class command sheets (uv commands, OS command cheat sheet). The source files are not stored in this repository.
>
> **🧪 What was taught:** the tools a Python developer needs and why. The interview questions are **independent preparation** built on these topics, not questions the instructor asked.

---

## 📚 What the instructor covered

### 🖥️ Tools

| Topic | Key point |
|---|---|
| 🤖 **First help** | Use an LLM for installation errors. Try to fix it yourself before asking in class |
| ⌨️ **UI vs CLI** | Anything you do with a mouse can be done from a terminal. Servers usually run **Linux with no screen**, so the terminal is the only way in |
| 🪟 **Windows terminals** | Command Prompt for daily work. PowerShell only when an install guide asks for it |
| 📝 **Editor vs IDE** | Notepad / nano only create and edit files (you still need nano on servers). **VS Code** = "hybrid IDE": editor + terminal + debugger + extensions. PyCharm is a full IDE but heavier. The course uses VS Code |

### 🐍 Three ways to get Python

| Path | What you get | Verdict |
|---|---|---|
| 1️⃣ python.org | **One** Python version | Fine, but switching versions means reinstalling |
| 2️⃣ Anaconda / Miniconda | `conda`: many versions + data-science tools | Works well, but **heavy** |
| 3️⃣ **uv** | Many versions, lightweight, very fast | ✅ **Course recommendation** |

> [!TIP]
> **Why multiple versions?** One team's product may have started years ago on an old Python while another team uses the current stable one. With a single install you would uninstall and reinstall every time you switch teams. A **virtual environment** gives each project its own Python.

### 📦 Local vs global environments (uv)

| | 🏠 Local | 🌍 Global (centralised) |
|---|---|---|
| **Create** | `uv venv --python 3.12` (inside the project) | `uv venv --python 3.12 <folder>` |
| **Lives in** | `.venv` next to the code | A folder you choose, outside projects |
| **Activate (Windows)** | `.venv\Scripts\activate` | `<folder>\Scripts\activate` |
| **Activate (macOS/Linux)** | `source .venv/bin/activate` | `source <folder>/bin/activate` |
| **Instructor used it for** | Projects and **deployment** | Learning and **day-to-day dev**, reused across projects |
| **VS Code finds it?** | ✅ Automatically | ❌ Must register the kernel (see below) |

**▶️ Run a file:** activate the environment, then `python file.py`. `uv run file.py` finds a local `.venv` by itself, but the instructor advised the standard method while learning.

### 📓 Notebooks and Colab

- **Jupyter in VS Code:** install the Jupyter extension, then pick a **kernel** (the Python executable).
- **Registering a global environment** so VS Code can see it:

  ```bash
  # with the global environment activated
  uv pip install ipykernel
  python -m ipykernel install --user --name=<env> --display-name="<label>"
  # then restart VS Code
  ```

- **Notebooks are for experiments, `.py` files for deployment.** Move only the finished code into `.py` files.
- **Google Colab:** a free notebook in the browser with no installs and an optional **GPU/TPU** runtime (daily limits). When the runtime resets, variables and files are lost but the notebook is kept. Prefix shell commands with `!`.

### 📝 Assignment

Practise the OS cheat sheet: `cd` · `cd ..` · `dir` / `ls -la` · `mkdir` · `rmdir` · creating empty files from the terminal.

### 🎙️ From the cohort Q&A (instructors' answers, summarised)

| | Point |
|:---:|---|
| 👑 | **FDE = stakeholder.** The one engineer who knows the whole system when something breaks. Some FDE work is on-site with a client. Without external clients, it means ownership |
| 🧪 | **Dev and test still exist.** Working "in production" with a client does not mean skipping them |
| ☁️ | **Cloud concepts transfer.** Understand servers (e.g. EC2), serverless, security and APIs on one cloud, and moving to another is mostly mapping names |
| 📄 | **`skills.md`-style files** guide coding assistants. Production agent systems are built in code (e.g. a ReAct agent with LangGraph) |
| 🐍 | **Keeping an old system Python is fine if you understand activation.** Confusion starts when `python` runs without an environment activated |
| 🏷️ | **Keep the default `.venv` name**, because tools look for it |

---

## ⏱️ 10-minute recap

A Python project needs three things to be repeatable:

| Need | Solved by |
|---|---|
| 🐍 A known Python version | uv / virtual environment |
| 📦 A known set of libraries | uv / pinned dependencies |
| ⌨️ A way to run it without a mouse | the terminal, the only way onto a server |

Notebooks are for exploring; code that ships lives in `.py` files.

> [!IMPORTANT]
> In an FDE job the customer's machine is never your laptop. **"It runs for me" is not done.**

## ⚖️ Key decisions and trade-offs

| Choice | ✅ Good when | ⚠️ Poor fit / risk |
|---|---|---|
| 🏠 Local `.venv` per project | Shipping, CI, deployment, handing code over | Disk use grows with each project |
| 🌍 Shared (global) env | Learning, quick experiments, many small scripts | One upgrade can break another project; hard to reproduce |
| 🐍 conda | Heavy scientific stacks with native libraries | Large install, slower |
| 📓 Notebook | Exploring data, testing prompts, demos | Hidden state from out-of-order cells; hard to test and review |
| ☁️ Colab | Need a GPU for a short time | Session loss; **customer data must not be uploaded without approval** |

> [!NOTE]
> **🧠 Independent note (not from the class):** most teams today pin dependencies **per project** (`pyproject.toml` + `uv.lock`, or a pinned `requirements.txt`) so every machine installs the same thing. A shared environment is convenient but is **not** a reproducibility tool.

---

## 💬 Q1. How do you set up a Python project so someone else, or a customer's server, can run it?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "I pin the Python version and the dependencies, keep them in the repository, and make the run steps one command. In my live products I use a pinned `requirements.txt` and a Docker image based on a slim Python image, so the server runs exactly what I tested. For a new project I would use uv with `pyproject.toml` and a lock file, because it is fast and gives the same environment with `uv sync`. I also keep secrets out of the repo, read config from environment variables, and run the tests in CI on a clean machine."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>Why Docker if you already have a virtual environment?</b></summary>

A virtual environment fixes **Python libraries**. Docker also fixes **OS packages and system libraries**, so the server matches what I tested.

</details>

<details>
<summary><b>What goes in the lock file vs <code>pyproject.toml</code>?</b></summary>

| File | Holds |
|---|---|
| `pyproject.toml` | The version **ranges** you allow |
| Lock file | The **exact** resolved versions |

</details>

<details>
<summary><b>How do you upgrade a dependency safely?</b></summary>

1. Upgrade **one at a time** in a branch
2. Run the tests and evaluations
3. Read the changelog for breaking changes
4. Deploy

</details>

<details>
<summary><b>What if the customer's server has no internet?</b></summary>

Build wheels or an image ahead of time and move them through the approved channel. **Confirm the process with their IT team first.**

</details>

---

## 💬 Q2. "It works on my laptop but fails on the customer's server." How do you debug it?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "First I make the failure visible: the exact error, the command and the environment. Then I compare the two machines step by step: which Python runs, whether the right environment is active, the library versions, environment variables and secrets, file paths, and OS differences such as Windows versus Linux. If I can, I reproduce it in a container that matches their server. I fix the cause, not the symptom, and add a check so it doesn't come back."

**🔎 Checklist to compare the two machines**

| Check | Command / place |
|---|---|
| Which Python? | `which python` · `python --version` |
| Right environment active? | prompt name, `sys.prefix` |
| Library versions | `pip freeze` vs lock file |
| Env vars and secrets | `.env`, hosting settings |
| Paths and OS | Windows `\` vs Linux `/`, case-sensitive files |

**❓ Follow-ups** · *click to open*

<details>
<summary><b>What's the most common cause you've seen?</b></summary>

The **wrong interpreter or environment**, then a **missing environment variable**. Use an example from your own deployments only if you can describe it truthfully.

</details>

<details>
<summary><b>You can't SSH into their server. Now what?</b></summary>

Ask for logs and the output of a small diagnostic script, and agree on a safe screen-share or support process.

</details>

<details>
<summary><b>How do you avoid this next time?</b></summary>

- The **same Docker image** in dev and prod
- A **pinned lock file**
- A **health/readiness endpoint** that checks dependencies at start-up
- A CI job on Linux

</details>

---

## 💬 Q3. When would you use a notebook, and when not?

> [!TIP]
> **🗣️ Say this (≈45 s)**
>
> "I use a notebook to explore: look at data, try prompts, compare options and show results to a stakeholder. Once the code works, I move it into `.py` modules with tests, because notebooks keep hidden state when cells run out of order and they are hard to review in Git. In production the service runs code from modules, not notebooks."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>How do you keep a notebook trustworthy?</b></summary>

**Restart the kernel and run all cells** before sharing, and keep reusable logic in imported modules.

</details>

<details>
<summary><b>A data scientist hands you a notebook to deploy. What do you do?</b></summary>

1. Pin its environment
2. Pull the logic into functions
3. Add tests with a few known inputs and outputs
4. Wrap it in an API or a job

</details>

---

## 💬 Q4. You must work on a locked-down Linux server with no UI. Are you comfortable?

> [!TIP]
> **🗣️ Say this (truthful pattern, ≈45 s)**
>
> "Yes for the basics: navigating files, reading logs, running services, checking processes and disk, and editing with a terminal editor. My products run on managed hosting, so for deeper server administration I would follow the customer's runbooks, ask for the permission model up front, and never make changes I cannot roll back."

> [!IMPORTANT]
> **✍️ Rav:** adjust the first sentence to your real Linux depth before using it.

**❓ Follow-ups** · *click to open*

<details>
<summary><b>How do you check why a service is down?</b></summary>

Process status → recent logs → disk and memory → recent deploys or config changes.

</details>

<details>
<summary><b>What do you never do on a customer server?</b></summary>

- ❌ Run unreviewed commands with admin rights
- ❌ Copy customer data to personal tools
- ❌ Make changes without telling the owner

</details>

---

## 💬 Q5. The customer needs a GPU for an experiment. Would you use Colab?

> [!TIP]
> **🗣️ Say this (≈60 s)**
>
> "For public or synthetic data, a hosted notebook is a quick way to test an idea. For customer data, I would first check their data policy. Usually the right answer is a GPU in their own cloud account or an approved environment, because uploading their data to a personal notebook could break their security rules. I would also measure whether we need a GPU at all. In Poultry 360 I chose CPU inference with an ONNX model because the hosting had no GPU."

**❓ Follow-ups** · *click to open*

<details>
<summary><b>How do you decide CPU vs GPU?</b></summary>

Latency target · request volume · model size · cost · what the deployment environment actually has.

</details>

<details>
<summary><b>What happens to work when the Colab runtime resets?</b></summary>

Variables and local files are **lost**. Save results to durable storage and keep the code in version control.

</details>

---

## 🏋️ Practical drill (15 minutes)

Without notes, on your own machine:

- [ ] Create a folder, then a local uv environment with a specific Python version, and activate it
- [ ] Install one library, write a two-line script that prints the Python version and the library version, and run it
- [ ] Explain aloud in **60 seconds** why a teammate would get the same result, and what extra step (lock file or Docker) makes it certain
- [ ] Delete the environment and **recreate it from your files alone**. If you can't, note what was missing

## 🧾 Candidate evidence to prepare

- 🐳 Poultry 360 and Aaina run as **Docker images** (slim Python base) with pinned `requirements.txt`. Be ready to explain why each is pinned and how you upgrade.
- 💻 Poultry 360's **CPU-only ONNX** choice (no GPU on free hosting) is a real environment-constraint story → [story bank](../../03-story-bank/README.md)

> [!IMPORTANT]
> **✍️ To verify:**
> - your honest depth with Linux server administration
> - Windows vs Linux differences you have actually hit
> - whether you have used uv or conda on a team

## 🔍 Open questions

- `pip` vs uv dependency management, lock files and `pyproject.toml` were not covered. Package installation comes later.
- Docker and deployment were not covered yet. Revisit Q1 when they are.

---

⬅️ [Session 00 — Induction](../2026-10-04-induction/README.md) · 🏠 [README](../../README.md) · ➡️ [Session 02 — Python fundamentals](../2026-10-11-python-fundamentals/README.md)

🔗 Related: [AI systems](../../02-question-banks/ai-systems.md) · [Customer discovery](../../02-question-banks/customer-discovery.md)
