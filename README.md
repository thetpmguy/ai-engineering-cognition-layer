# ai-engineering-cognition-layer

# AI Engineering Cognition Layer – Reference Implementation

This repository is a reference implementation of two practical, production-minded patterns for using AI to improve engineering delivery in complex systems:

1. **VScode + Claude as a constrained engineering cognition layer**
2. **GitHub Copilot as a structured productivity and discipline amplifier**

This is **not** a demo of “AI writing code.”
This is a demonstration of how AI can be embedded into **engineering workflows with guardrails, metrics, and accountability**, so improvements show up in lead time, PR quality, and incident trends — not just developer sentiment.

---

## Why this exists

Modern engineering teams don’t fail because they lack talent.
They fail because systems accumulate **cognitive debt**:

- Domain logic spreads across services, configs, and tribal knowledge
- Senior engineers become human compilers and reviewers
- PRs bounce for structural reasons (tests, logging, conventions)
- Onboarding depends on who you can ask, not what’s documented

AI can help — but only when treated as **part of a system**, not a free-form autocomplete toy.

This repo shows how.

---

## Two patterns demonstrated

### Case 1: VScode + Claude as an Engineering Cognition Layer

**Goal:** Reduce planning, discovery, and review friction  
**Approach:**  
- Strict prompt workflows with enforced schemas
- Retrieval over internal standards and runbooks (mocked here)
- CI + PR integration that cites policy, not opinion
- Governance: redaction, allowlists, logging, model fallback

VScode is treated as an **interaction surface**, not an authority.
Deterministic systems remain in control.

📁 See: `case-studies/case-1-VScode-claude-cognition-layer/`

---

### Case 2: GitHub Copilot as a Discipline Amplifier

**Goal:** Reduce repetitive engineering work and improve consistency  
**Approach:**  
- Heavy use of repo templates and examples
- Copilot assists with boilerplate, tests, logging
- CI enforces standards deterministically
- Metrics drive iteration

Copilot accelerates execution **after standards already exist**.
It does not replace engineering judgment.

📁 See: `case-studies/case-2-copilot-discipline-amplifier/`

---

## Architecture (high level)

```text
Engineer
  ↓
IDE (VScode) or GitHub (Copilot)
  ↓
Opinionated templates + schemas
  ↓
Deterministic CI gates
  ↓
Optional AI explanations (policy-cited only)
  ↓
Measured delivery + quality outcomes
