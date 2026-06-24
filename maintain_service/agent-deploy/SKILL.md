---
name: agent-deploy
description: >
  Safely transition an agent from development to production using pre-flight
  checks, appropriate rollout strategies, and a defined rollback plan. Use this
  skill before deploying any agent change to a production environment — whether
  it's a new agent, a prompt change, a tool update, or a configuration change.
  Trigger phrases: "deploy this agent", "push this to production", "release this
  change", "how do we roll this out safely", "what do we check before deploying",
  "we're ready to go live", "deploy to staging/prod", "release strategy for this".
  Agent deployment is not like deploying code — a broken agent can silently
  produce wrong outputs at scale before anyone notices. This skill prevents that.
---

# Agent Deploy Skill

Deploys agent changes safely — with pre-flight validation, the right rollout
strategy for the risk level, and a tested rollback path before going live.

## Core Principle

> Never deploy without knowing how to un-deploy.
> The cost of a bad rollout is proportional to how long it takes to detect
> and how hard it is to revert. Minimize both before starting.

Agent failures are different from service failures:
- The service stays up — so no automatic alerts fire
- Output is wrong, not absent — users may not notice immediately  
- Wrong outputs may already have been acted on before detection
- Prompt changes are invisible to standard infrastructure monitoring

---

## Pre-Flight: The Deployment Contract

Before any deployment, three things must exist:

```
□ ROLLBACK PLAN — documented and tested before going live
  "If this deployment causes problems, we will: <exact steps>"
  If rollback hasn't been tested → do not deploy

□ DETECTION METHOD — how will we know if something is wrong?
  "We will know this deployment is failing when: <observable signal>"
  If there's no way to detect failure → do not deploy

□ SCOPE OF CHANGE — exactly what is changing
  "This deployment changes: <list>"
  "This deployment does NOT change: <list>"
  If scope is unclear → use agent-qa PRE mode to define it first
```

---

## Deployment Risk Assessment

Not every deployment needs the same level of caution.
Assess risk before choosing a rollout strategy:

```
RISK FACTORS — each adds to deployment risk:
  □ First deployment of this agent (no production baseline)      +HIGH
  □ Prompt changes (invisible to infra monitoring)               +MEDIUM
  □ Tool changes (external service connections modified)         +HIGH
  □ Model version change (behavior may differ subtly)            +HIGH
  □ Context window or memory changes                             +MEDIUM
  □ Changes to irreversible actions (send, delete, publish)      +HIGH
  □ No staging environment was used                              +HIGH
  □ No evaluation results from agent-audit / anti-hallucination  +MEDIUM
  □ High traffic volume (many users affected if wrong)           +MEDIUM

RISK LEVELS:
  0–1 factors → LOW RISK   → Full rollout acceptable
  2–3 factors → MEDIUM     → Canary or gradual rollout
  4+ factors  → HIGH RISK  → Shadow mode first, then canary
```

---

## Rollout Strategies

Choose based on risk level. See `references/rollout-strategies.md` for detail.

```
SHADOW MODE (High Risk — new agents, major changes)
  New agent runs in parallel with existing agent.
  New agent's output is logged but NOT used.
  Compare outputs. Validate quality. Then promote.

  Use when:
    - First production deployment of a new agent
    - Major prompt or architecture change
    - Model version change
    - Any change where you're not confident in quality

CANARY (Medium Risk — incremental changes)
  Route a small % of traffic (5–10%) to the new version.
  Monitor quality metrics for 24–48 hours.
  Expand if metrics are stable; rollback if they degrade.

  Use when:
    - Prompt refinements with clear intent
    - Tool parameter changes
    - Configuration updates

GRADUAL (Medium Risk — known-good changes with scale concern)
  10% → 25% → 50% → 100% over multiple days.
  Each stage requires explicit approval before expanding.
  
  Use when:
    - Change is validated in staging but traffic is high
    - Team wants manual control at each stage

FULL CUTOVER (Low Risk only)
  Switch all traffic immediately to the new version.
  Only acceptable when: change is minor + rollback is instant.
  
  Use when:
    - Bug fix with clear test coverage
    - Config-only change with no behavior impact
    - Rollback is a single config toggle
```

---

## Deployment Workflow

### Step 1 — Pre-Flight Checklist

Run the checklist in `references/deploy-checklist.md` for the target environment.

Core gates (must all pass before proceeding):
```
QUALITY GATES
□ agent-audit passed on the new version's output samples
□ anti-hallucination check run if agent handles data/numbers
□ Resolution rate (or equivalent metric) ≥ baseline on eval set
□ No new Critical or High defects introduced

SAFETY GATES  
□ Rollback procedure documented and tested in staging
□ All irreversible tool actions identified and gated
□ Scope of change defined; agent-qa PRE mode approved

OPERATIONAL GATES
□ Deployment window chosen (avoid peak traffic hours)
□ On-call person identified for the deployment window
□ Detection method confirmed: how will failure be detected?
□ Alert thresholds set in agent-monitor (or being set now)
```

If any gate fails → do not proceed. Fix the gate first.

### Step 2 — Staging Validation

Every deployment goes to staging before production.
Staging must be a realistic replica — not a toy environment.

```
In staging, verify:
□ Agent starts and accepts requests without error
□ All tool connections are live (use staging versions of external services)
□ Run the standard eval set: metrics match or exceed baseline
□ Run at least 1 adversarial test case (unexpected input)
□ Simulate a failure: confirm rollback works as documented
□ Confirm monitoring is capturing the right signals
```

### Step 3 — Execute Rollout

Follow the chosen strategy from `references/rollout-strategies.md`.

At each stage gate, evaluate:
```
EXPAND if:
  □ Error rate unchanged or lower
  □ Output quality metrics stable or improved
  □ Cost per task within expected range
  □ No unexpected tool call patterns

HOLD if:
  □ Any metric has degraded but not clearly from the deployment
  → Investigate before expanding; do not expand uncertainty

ROLLBACK if:
  □ Error rate increased significantly (>2× baseline)
  □ Quality metric dropped below threshold
  □ Unexpected behavior observed in production outputs
  □ Cost per task increased significantly (>50%)
  → Execute rollback plan immediately; investigate from logs
```

### Step 4 — Post-Deploy Validation

After full rollout, run a structured validation period:

```
T+1 hour:   Check error rates and spot-check 5 output samples
T+24 hours: Review full quality metric suite; compare to pre-deploy baseline
T+72 hours: Extended review; confirm no slow-building degradation
T+1 week:   Formal sign-off; update baseline metrics for future deployments
```

Document the outcome:
```
DEPLOYMENT RECORD
  Version deployed:  <identifier>
  Changes included:  <list>
  Rollout strategy:  <shadow/canary/gradual/full>
  Staging results:   <metrics>
  Production result: [SUCCESS | PARTIAL | ROLLED BACK]
  Baseline updated:  <new baseline metrics>
  Lessons learned:   <what to do differently next time>
```

---

## Rollback Execution

When rollback is triggered:

```
Step 1 — Stop the bleeding immediately
  Revert traffic to previous version (config change, feature flag, or redeploy)
  Target: rollback complete within 5 minutes of decision to roll back

Step 2 — Assess impact
  How long was the bad version live?
  How many requests were processed?
  Were any irreversible actions taken (emails sent, data modified)?

Step 3 — Preserve evidence
  Save logs from the bad deployment before anything is overwritten
  Export sample outputs that demonstrate the problem

Step 4 — Communicate
  Notify stakeholders of rollback and reason
  Set expectation for when investigation will complete

Step 5 — Root cause before re-deploying
  Run agent-audit on the rolled-back version's outputs
  Identify the specific cause of failure
  Fix + validate in staging before attempting production again
```

---

## Reference Files

- `references/rollout-strategies.md` — Full strategy playbooks with monitoring triggers
- `references/deploy-checklist.md` — Environment-specific pre-flight checklists

---

## Handoffs

```
FROM project-planner:   receives task completion confirmation + eval results
FROM agent-audit:       receives quality gate results
FROM anti-hallucination: receives output trust rating
TO agent-monitor:       hands off baseline metrics + alert thresholds to watch
```
