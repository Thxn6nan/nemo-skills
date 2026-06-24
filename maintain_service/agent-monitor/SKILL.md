---
name: agent-monitor
description: >
  Track agent health in production using quality-first metrics, defined alert
  thresholds, degradation patterns, and structured response playbooks. Use this
  skill after deploying an agent (agent-deploy) to set up ongoing monitoring,
  or when an agent in production shows unexpected behavior. Trigger phrases:
  "set up monitoring for this agent", "the agent seems to be degrading",
  "what should we measure in production", "agent quality is dropping",
  "set alert thresholds", "how do we know if the agent is working well",
  "review agent health", "something changed in agent behavior", "monitor this".
  Standard infrastructure monitoring (uptime, latency, errors) is necessary
  but not sufficient for agents. This skill adds what infra monitoring misses:
  output quality, cost drift, hallucination rate, and scope creep over time.
---

# Agent Monitor Skill

Maintains ongoing visibility into agent health in production.
Catches quality degradation, cost drift, and behavioral change before
they cause significant harm — not after users report problems.

## Core Principle

> Agent monitoring is not the same as service monitoring.
> A service that returns 200 OK is healthy.
> An agent that returns 200 OK with a wrong answer is not.
> Quality must be measured continuously, not just at deploy time.

---

## The Four Monitoring Layers

Standard infrastructure monitoring covers Layer 1 only.
All four are required for agents:

```
Layer 1: INFRASTRUCTURE (standard — most teams already have this)
  Uptime, error rate, latency, throughput
  → Caught by: existing APM, logs, dashboards

Layer 2: QUALITY (agent-specific — usually missing)
  Output correctness, task completion rate, user outcomes
  → Caught by: eval sampling, outcome tracking, user feedback

Layer 3: TRUST (agent-specific — almost always missing)
  Hallucination rate, citation accuracy, data fidelity
  → Caught by: anti-hallucination sampling, source verification

Layer 4: BEHAVIOR (agent-specific — advanced)
  Scope drift, tool call patterns, decision consistency
  → Caught by: agent-qa sampling, tool call auditing
```

---

## Setup Workflow (Use Once, Post-Deploy)

### Step 1 — Establish Baselines

Baselines must be set from the deployment sign-off (agent-deploy output).
Without a baseline, you cannot detect degradation.

```
BASELINE METRICS REQUIRED:
  From agent-deploy sign-off:
    □ Quality score at T+24h post-deploy
    □ Error rate at T+24h post-deploy
    □ Cost per task at T+24h post-deploy
    □ Hallucination rate at T+24h post-deploy (if measured)

  From ongoing operation (establish in first 2 weeks):
    □ Task completion rate: % of tasks fully completed vs. abandoned/escalated
    □ Tool call rate: average number of tool calls per task
    □ P95 latency: 95th percentile response time
    □ Weekly cost total: for budget trending

  BASELINE FORMAT:
    Metric              Value    Acceptable range    Alert threshold
    ─────────────────── ──────── ─────────────────── ───────────────
    Quality score        XX%      ≥ baseline − 5pp    < baseline − 10pp
    Error rate            X%      ≤ baseline × 1.5    > baseline × 2
    Cost per task         $X      ≤ baseline × 1.2    > baseline × 1.5
    Hallucination rate    X%      ≤ baseline + 2pp    > baseline + 5pp
    Completion rate      XX%      ≥ baseline − 3pp    < baseline − 8pp
```

### Step 2 — Configure Alert Thresholds

See `references/metrics-guide.md` for threshold recommendations by metric type.

Alert levels:
```
WATCH  — metric approaching threshold; increase sampling; no action yet
ALERT  — threshold crossed; investigate immediately; may need action
PAGE   — critical threshold crossed; wake someone up; act now
```

Rule: alerts should be actionable. If an alert fires and the response is
"wait and see" — raise the threshold. Alerts that are ignored train teams
to ignore all alerts.

### Step 3 — Set Sampling Schedule

Full output review is expensive. Use sampling:

```
AUTOMATED (run continuously):
  □ Error rate — every request
  □ Latency — every request
  □ Cost per task — every request
  □ Tool call count — every request

SAMPLED (run on a % of requests):
  □ Quality score — sample 5–10% of outputs through eval rubric
  □ Hallucination check — sample 5% of data-containing outputs
  □ Scope check — sample 2% of outputs through agent-qa POST mode

SCHEDULED (run on a cadence):
  □ Full eval set — weekly (same set used pre-deploy)
  □ Manual output review — weekly, 10–20 samples
  □ Cost trend review — weekly
  □ Drift analysis — monthly (compare current behavior to baseline)
```

### Step 4 — Define Review Cadence

```
DAILY (automated dashboard check, 5 minutes):
  □ Error rate within range?
  □ Any alerts fired in last 24h?
  □ Cost within daily budget?

WEEKLY (15–30 minutes):
  □ Run standard eval set; compare to baseline
  □ Review 10–20 sampled outputs manually
  □ Review cost trend: is it flat, rising, or volatile?
  □ Check tool call patterns: anything unusual?
  □ Acknowledge or close any open Watch/Alert items

MONTHLY (1 hour):
  □ Full drift analysis: compare current metrics to 30/60/90 day trend
  □ Review hallucination rate trend
  □ Review user feedback patterns (support tickets, ratings)
  □ Decision: does baseline need to be updated? (performance improved)
  □ Decision: does any metric trend require action?
```

---

## Ongoing Monitoring Workflow

### When Metrics Are Stable

```
Stable = all metrics within acceptable range, no trends

Action: maintain current cadence; no additional investigation needed.
Review cadence may be relaxed after 30 days of stability.
```

### When a Watch Condition is Triggered

```
WATCH: metric approaching threshold but not yet crossed

Actions:
  □ Increase sampling rate for that metric
  □ Add manual review of outputs from the last 24h
  □ Check for recent changes: prompt updates, tool changes, traffic spikes
  □ Document in monitoring log: what was observed, when, what was checked

Do NOT: make changes to the agent yet. Observe first.
```

### When an Alert Fires

```
ALERT: metric has crossed the threshold

Immediate actions (within 1 hour):
  □ Confirm the alert is real (not a monitoring bug or data pipeline issue)
  □ Identify the scope: when did this start? How many requests affected?
  □ Check for recent deployments or external changes

Investigation:
  □ Pull sample outputs from the alert period
  □ Run agent-audit on the sample
  □ Run anti-hallucination on data-containing outputs
  □ Compare tool call patterns to baseline
  □ Check if any external dependency (API, model, data source) changed

Decision:
  □ Root cause found + fixable → apply fix; monitor recovery
  □ Root cause found + requires deployment → run agent-deploy with fix
  □ Root cause not found → escalate to PAGE level; consider rollback
  □ False alarm → document; adjust threshold if consistently false
```

### When PAGE Level is Reached

```
PAGE: critical threshold crossed; agent behavior unacceptable in production

Immediate actions (within 15 minutes):
  □ Assess rollback: is rolling back to previous version the right call?
  □ If irreversible actions are being taken wrongly → halt immediately
  □ Notify stakeholders

Rollback decision:
  □ Can the impact be contained without rollback? (rate limit, partial disable)
    YES → contain, then investigate
    NO  → execute rollback from agent-deploy rollback plan

Post-incident:
  □ Preserve logs and output samples before rollback
  □ Run full root cause analysis before re-deploying
  □ Review whether alert thresholds should have caught this sooner
```

---

## Skill Trigger Points from Monitoring

Monitoring is the early warning system for the other skills:

```
Quality metric drops significantly    → trigger agent-audit
Hallucination rate rises              → trigger anti-hallucination (DETECT mode)
Scope drift score increases           → trigger agent-qa (POST mode)
Cost per task rises sharply           → trigger agent-engineering (Efficiency + Cost)
Direction feels wrong after 30 days   → trigger project-compass
Baseline has improved meaningfully    → trigger agent-deploy (update baseline)
```

---

## Reference Files

- `references/metrics-guide.md` — What each metric means and threshold recommendations
- `references/alert-response-guide.md` — Response playbook for each alert type

---

## Handoffs

```
FROM agent-deploy:    receives baseline metrics + initial thresholds
TO agent-audit:       escalates when quality metric drops
TO anti-hallucination: escalates when hallucination rate rises
TO agent-qa:          escalates when scope drift detected
TO project-compass:   escalates when 30-day trend suggests direction change needed
```
