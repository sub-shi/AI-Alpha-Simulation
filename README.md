# Alpha Generation Assistant (WorldQuant Brain + LLM)

Short overview
----------------
This repository contains notebooks and helper utilities to fetch WorldQuant Brain alphas, generate natural-language descriptions using an LLM, and synthesize new alpha expressions programmatically. The main artifacts are:

- `llm_competition.ipynb` — primary notebook that authenticates to WorldQuant Brain, queries alpha details, asks an LLM for descriptions, and attempts to generate and simulate new alphas using `ace_lib.py` helper functions.
- `ace_lib.py` — helper library that wraps WorldQuant Brain API calls and simulation helpers.
- `helpful_functions.py` — utility helpers used by the notebook/library.
- `requirements.txt` — Python dependencies.

Security & secrets
-------------------
- Do NOT commit API keys, passwords, or other secrets. This repo's `.gitignore` already excludes `ace.log`, `feedback_memory.json`, and a `secrets/` folder — keep credentials out of source control.
- Move any inline keys from notebooks or Python files into environment variables before pushing.

Recommended environment variables
---------------------------------
Set these in your shell (PowerShell examples):

```powershell
# For WorldQuant Brain (used by ace_lib.start_session)
$env:BRAIN_CREDENTIAL_EMAIL = "you@example.com"
$env:BRAIN_CREDENTIAL_PASSWORD = "<your-password>"

# Optional: override Brain API base URL
$env:BRAIN_API_URL = "https://api.worldquantbrain.com"

# For LLM providers used in notebooks (don't hardcode these):
$env:GENAI_API_KEY = "<your-google-genai-key>"
# or
$env:GROQ_API_KEY = "<your-groq-key>"
```

To persist environment variables permanently on Windows use `setx` (then restart your shell):

```powershell
setx BRAIN_CREDENTIAL_EMAIL "you@example.com"
setx BRAIN_CREDENTIAL_PASSWORD "<your-password>"
```

Quick setup
-----------
1. Create an isolated virtual environment and install dependencies (PowerShell):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

2. Export your environment variables (see previous section).

3. Open `llm_competition.ipynb` in Jupyter (or JupyterLab / VS Code) and run the cells.

Notes about running
-------------------
- The notebook uses `nest_asyncio` to allow nested event loops inside notebooks.
- The heavy operations (API calls and simulations) require valid Brain credentials and may be rate limited by the Brain API. If you see HTTP 429 responses, add sleeps or retry/backoff logic before re-running.
- The notebook will call out to an LLM (Gemini/Groq). Keep those calls disabled or mocked if you only want to run local simulations.

Common troubleshooting
----------------------
- ValueError / KeyError when manipulating pandas DataFrames: check whether API responses returned expected schema. Add defensive checks before indexing columns.
- TypeError about duplicate arguments calling `ace.generate_alpha`: call it with keyword `regular=...` rather than passing a session object as first positional argument.
- 429 Too Many Requests: add exponential backoff and jitter or reduce request rate.

Next steps (suggested)
----------------------
1. Remove any hard-coded API keys from notebooks and move them into environment variables.
2. Run `git status` locally, verify only intended files will be tracked, then commit and push.
3. Optionally, add a small script to run `black`/`ruff` or to validate secrets are not accidentally committed.

Contact / help
--------------
If you'd like, I can:
- Scan the repo for likely secrets (API keys) and report locations.
- Create a small helper script to load env vars and run a dry-run of `main()` with API calls mocked.

