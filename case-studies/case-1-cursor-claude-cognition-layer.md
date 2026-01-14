# Case 1: Cursor + Claude as an Engineering Cognition Layer

This case study demonstrates how Cursor was used as a **structured cognition surface** for engineers working in a complex, multi-domain codebase.

Cursor was explicitly **not** allowed to:
- Invent patterns
- Make architectural decisions
- Act autonomously

---

## Golden paths

### 1. Ticket → Plan → Patch

Output must include:
- Implementation plan
- Impacted services
- Code changes
- Test plan
- Rollout and rollback notes
- Citations to internal docs

Schema enforced via JSON.

### 2. Explain this system

Used for onboarding and debugging.
Responses must include:
- Step-by-step flow
- Services and events involved
- Known failure modes
- Links to runbooks and ADRs

### 3. Fix failing CI job

AI provides:
- Suspected root cause
- Smallest safe fix
- Verification steps
- Risk warnings for sensitive domains

---

## Key insight

This system reduced **thinking overhead**, not engineering rigor.
