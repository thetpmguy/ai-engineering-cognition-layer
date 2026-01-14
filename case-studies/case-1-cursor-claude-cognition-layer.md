# Case 1: Cursor + Claude as an Engineering Cognition Layer


## S — Situation

I was working with a multi-region e-commerce retailer operating web and mobile storefronts across NA, EMEA, and APAC. We had roughly 120 engineers split across checkout, catalog, fulfillment, identity, and platform teams.  

The stack was modern but dense:
- Frontend: React / Next.js  
- Backend: Node.js, Java, Kotlin services  
- Architecture: event-driven integrations  
- Platform: Kubernetes, Terraform  
- Observability & Ops: Datadog, GitHub Actions, PagerDuty  

The problem wasn’t that engineers were bad or slow. The problem was that the system had accumulated **cognitive debt**.

Checkout logic alone had dozens of edge cases: promotions, shipping rules, tax, fraud hooks, currency conversions, feature flags, and regional compliance. Knowledge lived in tribal form. Senior engineers were acting as human compilers, translators, and reviewers.

Delivery metrics were slipping:
- PRs bounced back and forth for structural reasons
- New hires took almost two weeks before making a meaningful contribution
- Debugging required spelunking across logs, traces, event payloads, and configs
- Teams felt busy, but not effective

Leadership kept hearing “we need to move faster,” but without a concrete lever to pull.

---

## T — Task

My task was **not** “introduce AI.”

My task was to reduce engineering friction **without lowering quality**, and to do it in a way that was:
- Provable  
- Auditable  
- Safe for a regulated e-commerce environment  

That meant:
- Shortening the path from ticket → correct code  
- Reducing repetitive copy/paste engineering  
- Improving PR quality *before* human review  
- Making debugging and onboarding less dependent on senior engineers  
- Measuring improvement using delivery and quality metrics, not vibes  

Crucially, this could not turn into random AI autocomplete. It had to behave like a **consistent internal engineering assistant**, aligned to our architecture and standards.

---

## A — Action

### Step 1: Establish a real baseline (before touching AI)

Before any rollout, we captured six weeks of baseline data. This was non-negotiable.

Baseline metrics:
- Median lead time for change (merge → prod): **6.5 days**
- PR cycle time (open → merge): **2.4 days**
- Review iterations per PR: **3.1**
- PRs with missing or weak tests: **28%**
- Regression incidents: **5–7 per month**
- New hire time to first meaningful PR: **~12 business days**

This gave us a clean “before” picture.

---

### Step 2: Decide what Cursor was and was not

We standardized on Cursor as the IDE, but we explicitly decided that Cursor would **not** be a free-form chat toy.

Cursor was treated as:
- An interaction surface  
- Not the source of truth  
- Not autonomous  
- Not allowed to invent patterns  

The intelligence lived in how we constrained it.

---

### Step 3: Build the knowledge layer (RAG)

We created an internal retrieval layer so the model would reason using *our system*, not generic internet code.

We ingested:
- Architecture Decision Records (ADRs)
- Checkout, promo, tax, and shipping runbooks
- API contracts and domain models
- Incident postmortems
- A domain glossary to prevent semantic drift (e.g., refund vs exchange vs return)

These documents were chunked, embedded, and indexed in a vector store. Cursor queries were routed through this index so responses cited internal sources instead of hallucinating.

This step alone eliminated a large amount of confusion in system explanations.

---

### Step 4: Ship opinionated prompt + workflow templates

We deliberately avoided “just chat with the codebase.”

Instead, we shipped **three golden paths**, each with enforced schemas.

#### A) Ticket → Plan → Patch

The engineer selected a Jira ticket and the impacted repo. Cursor produced:
- A short, structured implementation plan
- Explicit services and modules touched
- A code diff
- A test plan
- Rollout and rollback notes  

If any section was missing, the output was considered invalid.

---

#### B) Explain this system

The engineer highlighted files and asked questions like:
> “How does checkout reservation work?”

The output had to include:
- A step-by-step flow
- Referenced services and events
- Links to ADRs and runbooks
- Known failure modes

---

#### C) Fix this failing CI job

Engineers pasted CI logs and selected relevant files. Cursor returned:
- A suspected root cause
- The smallest safe patch
- Verification steps
- A warning if the change touched risky domains like payments or promotions  

These were structured artifacts, not suggestions.

---

### Step 5: Integrate with CI and PRs

We extended this into GitHub Actions.

The CI pipeline:
- Labeled PRs likely missing tests
- Flagged missing observability fields (correlation IDs, structured logs)
- Identified risky changes in checkout or payment modules
- Optionally ran an “AI reviewer” that commented **only** when citing internal standards, never opinion  

This was key: the AI reviewer was not subjective. It spoke only when it could point to an ADR or policy.

---

### Step 6: Governance and security

We shipped guardrails from day one:
- No secrets or customer PII allowed in prompts
- Automatic redaction
- Allowed repositories only
- Prompt and output logging for quality review
- Model fallback for availability  

This turned AI usage into an **auditable system**, not a shadow practice.

---

## R — Result

We ran the system for 8–12 weeks and measured the same KPIs.

The results were not magical, but they were consistent and defensible.

### Delivery and quality
- PR cycle time dropped from **2.4 days → 1.5 days** (~37%)
- Review iterations per PR dropped from **3.1 → 2.0**
- PRs with weak tests dropped from **28% → 12%**
- Lead time for change dropped from **6.5 days → 4.7 days**

### Operations
- Regression incidents dropped from **5–7/month → 3–4/month**
- MTTR for known issue classes (cache, promo, tax) improved by **20–30%**

### Onboarding
- Time to first meaningful PR dropped from **~12 days → ~7 days**

The credibility move mattered most.

We never claimed “AI wrote X% of our code.”

We showed:
- DORA trends
- GitHub analytics
- CI test quality metrics
- Incident trend analysis  

The story was simple and honest:

**We removed friction from planning, repetition, and review — without lowering standards.**
