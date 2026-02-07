# Session Variables

Fill these in before starting a workflow cycle. Referenced throughout all playbook prompts.

See [Playbook §6](llm-development-playbook.md#6-project-setup) for full context.

---

## Project Variables

> Defined once per project, updated rarely. (§6.1)

| Variable | Description | Value |
|----------|-------------|-------|
| `PROJECT_NAME` | Project identifier | |
| `REPO_URL` | Repository location | |
| `INTERNAL_DOCS_DIR` | Where specs and review outputs are stored | |
| `TEST_COMMAND` | How to run the test suite | |
| `SMOKE_TEST_COMMANDS` | Quick verification commands (project-specific CLI checks) | |
| `ARCHITECTURE_CONSTRAINTS` | Layer rules, import restrictions, etc. | |
| `REFERENCE_DOCS` | Paths to roadmap, architecture, strategy docs | |
| `BRANCH_CONVENTION` | Branch naming pattern | |
| `MODEL_ASSIGNMENTS` | Which model plays which role (see §3.1 for defaults) | |
| `DEVELOPER_TOOL` | CLI tool the developer agent uses | |

---

## Release Variables

> Decided per release cycle before entering the workflow. (§6.2, §6.3)

| Variable | Description | Value |
|----------|-------------|-------|
| **Current version** | Release version being worked on | |
| **Phase variant** | Feature / Patch / Hotfix (§15) | |
| **Pre-Phase A needed?** | Triage / Proposal Review / Neither (§5.3) | |
| **Track splitting?** | Monolithic / Track-based (§15.4) | |
| **Number of tracks** | 2–4, one per independent work stream | |
| **Branch name** | Exact branch name — propagates across all prompts | |
| **Base branch** | Usually `main` | |
| **PR number** | When exists — propagates across all prompts | |
| **Spec version** | Current approved spec version | |
| **Files in scope** | Focuses reviewers, especially for track-based work | |

---

## Example (filled in)

```
# Project Variables
PROJECT_NAME        = "acme-api"
REPO_URL            = "https://github.com/acme/acme-api"
INTERNAL_DOCS_DIR   = ".project-internal/"
TEST_COMMAND        = "pytest tests/ -v"
SMOKE_TEST_COMMANDS = "python -m acme --version && python -m acme validate --dry-run"
ARCHITECTURE_CONSTRAINTS = "Core must not import from UI; all CLI commands return exit codes 0-3"
REFERENCE_DOCS      = "docs/architecture.md, docs/roadmap.md"
BRANCH_CONVENTION   = "feature/v{version}-{description}"
MODEL_ASSIGNMENTS   = "CA: Claude, Peer: Gemini, Alignment: Claude, Adversarial: Codex, Dev: Codex"
DEVELOPER_TOOL      = "Codex CLI"

# Release Variables
Current version     = "2.1.0"
Phase variant       = "Feature"
Pre-Phase A needed? = "Neither"
Track splitting?    = "Monolithic"
Number of tracks    = "—"
Branch name         = "feature/v2.1.0-auth-refactor"
Base branch         = "main"
PR number           = "#60"
Spec version        = "v1.1"
Files in scope      = "src/auth/, src/middleware/auth.ts, tests/auth/"
```
