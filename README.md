# LLM Development Playbook

A lightweight system for solo developers to build software with LLM agents.

## What This Is

Three files that help you orchestrate multiple LLMs to write specs, code, and review each other's work:

| File | Audience | Purpose |
|------|----------|---------|
| `llm-development-playbook.md` | LLMs | Roles, workflows, prompts |
| `orchestrator-handbook.md` | You | Emergencies, escalation, monitoring |
| `session-variables.md` | Both | Your current session values |

## How It Works

You are the orchestrator. LLMs are your team. You trigger each handoff manually.

```
You write strategy
    ↓
LLM architects write specs (review each other until converged)
    ↓
LLM developer writes code
    ↓
LLM reviewers review (including adversarial reviewer who hunts bugs AND complexity)
    ↓
LLM synthesizers compress reviews for your decision
    ↓
You approve or request changes
    ↓
Merge
```

## Philosophy: Simplicity Over Cleverness

This workflow enforces **engineering constraints** at every stage to combat "sycophancy of complexity" — the tendency of LLMs to over-engineer solutions to demonstrate competence.

**Built-in constraints:**
- **YAGNI**: No generic interfaces until 3+ implementations exist
- **Boring Tech**: SQLite over Postgres, monolith over microservices, in-process over networked
- **Cut List**: Product Architect must explicitly defer non-essential features
- **ComplexityFlag**: Developer can REFUSE to implement over-engineered specs
- **Complexity Audit**: Adversarial Reviewer hunts unnecessary abstraction with the same rigor as bugs

Complexity is treated as a liability, not an asset.

## Quick Start

1. Fill in `session-variables.md` with your project details
2. Open a new LLM chat
3. Attach `llm-development-playbook.md` + `session-variables.md`
4. Paste the Generator Prompt from `orchestrator-handbook.md`
5. Get all your prompts generated with your variables substituted

## Key Concepts

- **Spec Convergence**: Multiple architect LLMs review specs until they agree — with Gate Checks that reject speculative design upfront
- **Adversarial Review**: One reviewer is prompted to reject by default — hunting both bugs AND architectural bloat
- **Model Diversity**: Use different models for developer vs. reviewers to avoid shared blind spots
- **Anti-Sycophancy**: Prompts force LLMs to take positions, not just validate
- **Side with the Pessimist**: When reviewers disagree on complexity, default is BLOCK

## Dependencies

Works with [Project Ontos](https://github.com/ohjona/Project-Ontos) for context persistence between agents. Not required, but recommended.

## Files

```
├── llm-development-playbook.md   # Give to LLMs
├── orchestrator-handbook.md      # Keep open for yourself
├── session-variables.md          # Edit per session
└── README.md                     # You are here
```

---

*Built for solo developers who want to ship, not babysit.*
