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
LLM reviewers review (including adversarial reviewer who tries to break it)
    ↓
LLM synthesizers compress reviews for your decision
    ↓
You approve or request changes
    ↓
Merge
```

## Quick Start

1. Fill in `session-variables.md` with your project details
2. Open a new LLM chat
3. Attach `llm-development-playbook.md` + `session-variables.md`
4. Paste the Generator Prompt from `orchestrator-handbook.md`
5. Get all your prompts generated with your variables substituted

## Key Concepts

- **Spec Convergence**: Multiple architect LLMs review specs until they agree
- **Adversarial Review**: One reviewer is prompted to find problems, not approve
- **Model Diversity**: Use different models for developer vs. reviewers to avoid shared blind spots
- **Anti-Sycophancy**: Prompts force LLMs to take positions, not just validate

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
