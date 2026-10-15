# Session 01 — Developer environment: CLI, VS Code, uv, notebooks (10 October 2026)

**Source:** supplied VTT transcript, `GMT20261010-022752_Recording.cutfile.20261010104927990.transcript.vtt`, and the class command sheets (uv commands, OS command cheat sheet). The source files are not stored in this repository.
**Session type:** first technical class (tools and installation), followed by a long cohort Q&A.
**What was actually taught:** the tools a Python developer needs and why: terminal vs UI, editor vs IDE, three ways to get Python, virtual environments with uv (local vs global), Jupyter notebooks in VS Code, and Google Colab. The interview questions below are **independent preparation** built on these topics. They are not questions the instructor asked.

## What the instructor covered

- **Use an LLM as your first help** for installation errors. Try to fix it yourself before asking in class.
- **UI vs CLI.** Every task you do with a mouse can be done from a terminal. Servers usually run Linux with no screen, so you reach them only through a terminal. On Windows the instructor preferred Command Prompt over PowerShell for daily work, and PowerShell only when an install guide asks for it.
- **Editor vs IDE.** A plain editor (Notepad, nano) only creates and edits files. You still need nano on Linux servers. VS Code was called a "hybrid IDE": editor + built-in terminal + debugger + extensions (Docker, cloud, CSV, notebooks). PyCharm is a full Python IDE but heavier. The course uses VS Code.
- **Three ways to get Python:**
  1. python.org installs **one** version.
  2. Anaconda/Miniconda gives `conda`, which manages many versions. It works well but is heavy.
  3. **uv** manages many versions and is lightweight. This is the course's recommended path. Existing installs do not need to be removed.
- **Why multiple versions:** one team's product was started years ago on an old Python, and another team uses a current stable one. With a single install you would uninstall and reinstall every time you switch teams. A virtual environment gives each project its own Python.
- **Local environment:** `uv venv --python 3.12` inside the project folder creates `.venv` next to the code. Activate it with `.venv\Scripts\activate` on Windows or `source .venv/bin/activate` on macOS/Linux. The instructor recommended local environments for projects and deployment.
- **Global (centralized) environment:** `uv venv --python 3.12 <folder of your choice>` creates the environment outside the project, so several projects with the same Python and libraries can reuse it. The instructor preferred this while learning and doing day-to-day development.
- **Run a file:** activate the environment, then `python file.py`. `uv run file.py` finds a local `.venv` automatically. The instructor advised the standard method while learning.
- **Notebooks (`.ipynb`):** install the Jupyter extension in VS Code, then choose a kernel (the Python executable). VS Code finds a local `.venv` on its own, but **not** a global one. Fix: activate the global environment, run `uv pip install ipykernel`, then `python -m ipykernel install --user --name=<env> --display-name=<label>`, and restart VS Code.
- **Use notebooks for experiments, `.py` files for deployment.** Move only the finished code into `.py` files.
- **Google Colab:** a free notebook in the browser with no installs and an optional GPU or TPU runtime with daily limits. When the runtime resets, variables and uploaded files are lost but the notebook is kept. Prefix shell commands with `!`.
- **Assignment:** practise the OS cheat sheet: `cd`, `cd ..`, `dir`/`ls -la`, `mkdir`, `rmdir`, and creating empty files from the terminal.

### From the cohort Q&A (instructors' answers, summarized)

- **FDE as stakeholder.** An FDE "owns" the outcome: the one engineer who knows the whole system when something breaks. Some FDE work is on-site in a client's environment. At companies without external clients it is about ownership more than client contact.
- **Dev and test still exist.** Working "in production" with a client does not mean skipping development and test environments.
- **Cloud concepts transfer.** If you understand servers (for example EC2), serverless, security and APIs on one cloud, moving to another cloud is mostly mapping names.
- **`skills.md`-style files** guide coding assistants. They are not how you build a production agent system, which you build in code (for example, a ReAct agent with LangGraph).
- **Keeping an old system Python is fine if you understand activation.** Confusion starts when you run `python` without activating an environment and get the wrong interpreter.
- **`.venv` naming:** keep the default name for local environments, because tools look for it.

## 10-minute recap

A Python project needs three things to be repeatable: a known Python version, a known set of libraries, and a way to run it without a mouse. uv gives the first two through virtual environments. The terminal gives the third, and it is the only way to work on a server. Notebooks are for exploring. Code that ships lives in `.py` files. In an FDE job, the customer's machine is never your laptop, so "it runs for me" is not done.

## Key decisions and trade-offs

| Choice | Good when | Poor fit / risk |
|---|---|---|
| Local `.venv` per project | Shipping, CI, deployment, handing code to someone else | Disk use grows with each project |
| Shared (global) environment | Learning, quick experiments, many small scripts with the same libraries | One upgrade can break another project. Hard to reproduce elsewhere |
| conda | Heavy scientific stacks with native libraries | Large install, slower |
| Notebook | Exploring data, testing prompts, showing results | Hidden state from out-of-order cells; hard to test and review |
| Colab | Need a GPU for a short time, no local setup | Session loss; **customer data must not be uploaded without approval** |

**Independent note (not from the class):** most teams today pin dependencies per project, with `pyproject.toml` plus a lock file (`uv.lock`) or a pinned `requirements.txt`, so the same install happens on every machine. A shared environment is convenient but is not a reproducibility tool.

## Interview question 1: How do you set up a Python project so someone else, or a customer's server, can run it?

**Ready answer:** "I pin the Python version and the dependencies, keep them in the repository, and make the run steps one command. In my live products I use a pinned `requirements.txt` and a Docker image based on a slim Python image, so the server runs exactly what I tested. For a new project I would use uv with `pyproject.toml` and a lock file, because it is fast and gives the same environment with `uv sync`. I also keep secrets out of the repo, read config from environment variables, and run the tests in CI on a clean machine."

**Follow-ups:**

- *Why Docker if you already have a virtual environment?* A virtual environment fixes Python libraries. Docker also fixes OS packages and system libraries, so the server matches what I tested.
- *What goes in the lock file vs `pyproject.toml`?* `pyproject.toml` has the ranges you allow, and the lock file has the exact resolved versions.
- *How do you upgrade a dependency safely?* Upgrade one at a time in a branch, run the tests and evaluations, read the changelog for breaking changes, then deploy.
- *What if the customer's server has no internet?* Build wheels or an image ahead of time and move them through the approved channel. Confirm the process with their IT team first.

## Interview question 2: "It works on my laptop but fails on the customer's server." How do you debug it?

**Ready answer:** "First I make the failure visible: the exact error, the command and the environment. Then I compare the two machines step by step: which Python runs (`which python`, `python --version`), whether the right environment is active, the library versions (`pip freeze` or the lock file), environment variables and secrets, file paths, and OS differences such as Windows versus Linux. If I can, I reproduce it in a container that matches their server. I fix the cause, not the symptom, and add a check, for example a start-up check or a CI job on Linux, so it does not come back."

**Follow-ups:**

- *Most common cause you have seen?* The wrong interpreter or environment, then a missing environment variable. Use an example from your own deployments only if you can describe it truthfully.
- *You cannot SSH into their server. Now what?* Ask for logs and the output of a small diagnostic script, and agree on a safe screen-share or support process.
- *How do you avoid this next time?* The same Docker image in dev and prod, a pinned lock file, and a health or readiness endpoint that checks dependencies at start-up.

## Interview question 3: When would you use a notebook, and when not?

**Ready answer:** "I use a notebook to explore: look at data, try prompts, compare a few options and show results to a stakeholder. Once the code works, I move it into `.py` modules with tests, because notebooks keep hidden state when cells run out of order and they are hard to review in Git. In production, the service runs code from modules, not notebooks."

**Follow-ups:**

- *How do you keep a notebook trustworthy?* Restart the kernel and run all cells before sharing, and keep reusable logic in imported modules.
- *A data scientist hands you a notebook to deploy. What do you do?* Pin its environment, pull the logic into functions, add tests with a few known inputs and outputs, then wrap it in an API or job.

## Interview question 4: You must work on a locked-down Linux server with no UI. Are you comfortable?

**Ready answer (truthful pattern):** "Yes for the basics: navigating files, reading logs, running services, checking processes and disk, and editing with a terminal editor. My products run on managed hosting, so for deeper server administration I would follow the customer's runbooks, ask for the permission model up front, and never make changes I cannot roll back."

**Follow-ups:**

- *How do you check why a service is down?* Process status, recent logs, disk and memory, then recent deploys or config changes.
- *What do you never do on a customer server?* Run unreviewed commands with admin rights, copy customer data to personal tools, or make changes without telling the owner.

*Rav: adjust the first sentence to your real Linux depth before using it.*

## Interview question 5: The customer needs a GPU for an experiment. Would you use Colab?

**Ready answer:** "For public or synthetic data, a hosted notebook is a quick way to test an idea. For customer data, I would first check their data policy. Usually the right answer is a GPU in their own cloud account or an approved environment, because uploading their data to a personal notebook could break their security rules. I would also measure whether we need a GPU at all. In Poultry 360 I chose CPU inference with an ONNX model because the hosting had no GPU."

**Follow-ups:**

- *How do you decide CPU vs GPU?* Latency target, request volume, model size, cost, and what the deployment environment actually has.
- *What happens to work when the Colab runtime resets?* Variables and local files are lost. Save results to durable storage and keep the code in version control.

## Practical drill (15 minutes)

Without notes, on your own machine:

1. Create a folder, then create a local uv environment with a specific Python version and activate it.
2. Install one library, write a two-line script that prints the Python version and the library version, and run it.
3. Explain out loud in 60 seconds why a teammate would get the same result, and what extra step (a lock file or Docker) makes it certain.
4. Delete the environment and recreate it from your files alone. If you cannot, note what was missing.

## Candidate evidence to prepare

- Poultry 360 and Aaina run as Docker images (slim Python base) with pinned `requirements.txt`. Be ready to explain why each is pinned and how you upgrade.
- Poultry 360's CPU-only ONNX choice (no GPU on free hosting) is a real environment-constraint story. See the [story bank](../../03-story-bank/README.md).
- *To verify:* your honest depth with Linux server administration, Windows vs Linux differences you have hit, and whether you have used uv or conda on a team.

## Open questions

- The class did not cover `pip` vs uv dependency management, lock files, or `pyproject.toml`. The instructor said package installation would come later.
- The class did not cover Docker or deployment yet. Revisit question 1 when it does.

Related: [AI systems and production judgment](../../02-question-banks/ai-systems.md) · [Customer discovery and delivery](../../02-question-banks/customer-discovery.md)
