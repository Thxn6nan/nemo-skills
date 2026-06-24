# Rollout Strategies — Detailed Playbooks

---

## Strategy 1: Shadow Mode

**Use for:** High-risk deployments. New agents. Major changes.

**Concept:** New agent runs alongside old agent. New agent's output is
logged and evaluated but never returned to users. No user impact possible.

```
SHADOW MODE SETUP:
  1. Deploy new agent to a shadow endpoint
  2. For every incoming request:
     a. Route to OLD agent → return output to user
     b. Simultaneously route to NEW agent → log output only
  3. Compare new vs old outputs daily
  4. Evaluate new agent quality against eval set

PROMOTION CRITERIA (when to switch from shadow → canary):
  □ Shadow has run for minimum 48 hours
  □ New agent quality metrics ≥ old agent on eval set
  □ No unexpected failure modes found in shadow logs
  □ Hallucination rate in shadow outputs ≤ old agent baseline
  □ Cost per task in shadow ≤ old agent × 1.2

WHAT TO COMPARE IN SHADOW LOGS:
  □ Did new and old agents reach the same conclusion on the same inputs?
  □ Where they differed: which was more correct?
  □ Did new agent use more or fewer tool calls?
  □ Any cases where new agent refused or failed that old agent handled?
  □ Any new error types in the new agent's logs?
```

---

## Strategy 2: Canary

**Use for:** Medium-risk changes. Validated prompt refinements. Tool updates.

**Concept:** Small % of real traffic goes to new version. Monitor quality
in production before committing to full rollout.

```
CANARY STAGES:
  Stage 1:  5% traffic → new version; 95% → old version
            Monitor for: 24 hours minimum
  Stage 2: 25% traffic → new version
            Monitor for: 24 hours
  Stage 3: 50% traffic → new version
            Monitor for: 12 hours
  Stage 4: 100% → full rollout

STAGE GATE CRITERIA (must pass before expanding):
  □ Error rate on new version: within 10% of old version
  □ Quality metric: within 5% of old version (or improved)
  □ Cost per task: within 20% of old version
  □ No Critical findings in manual output review
  □ P95 latency: within 20% of old version

IMPLEMENTATION OPTIONS:
  Option A: Feature flag (recommended)
    Set a flag: canary_percentage = 5
    Route based on: hash(request_id) % 100 < canary_percentage
    Rollback: set canary_percentage = 0

  Option B: Separate endpoint
    Route X% of load balancer traffic to new endpoint
    Rollback: set weight to 0%

  Option C: User cohort
    Assign specific users/sessions to canary group
    Better for measuring user-level outcomes
    Rollback: remove users from cohort
```

---

## Strategy 3: Gradual Rollout

**Use for:** Known-good changes where you want manual control at each stage.

```
GRADUAL SCHEDULE:
  Day 1:   10% — internal users or low-risk segment
  Day 2:   25% — if Day 1 clean
  Day 3:   50% — if Day 2 clean
  Day 5:  100% — if Day 3-4 clean

APPROVAL GATE between each stage:
  Required sign-off: <designated person>
  Required data: quality dashboard showing last 24h metrics
  Decision deadline: must decide expand/hold/rollback by <time>

HOLD CONDITIONS (stay at current % and investigate):
  □ Any metric degraded but root cause unclear
  □ Unusual volume of user complaints or support tickets
  □ External dependency showing elevated errors
  □ Team doesn't have capacity to monitor the next expansion
```

---

## Strategy 4: Full Cutover

**Use for:** Low-risk only. Bug fixes. Config-only changes.

```
PREREQUISITES (all must be true):
  □ Change is limited to: bug fix, config, or cosmetic prompt wording
  □ Rollback is: a single command or config toggle taking <2 minutes
  □ Eval set shows: no quality change (within noise)
  □ Risk assessment: 0–1 risk factors
  □ Deployment window: low-traffic period

VALIDATION IMMEDIATELY AFTER:
  T+5 min:  Confirm error rate unchanged
  T+15 min: Spot-check 3 live outputs manually
  T+1 hour: Confirm quality metrics stable
```

---

## Rollback Speed Targets

The longer a bad agent runs, the more damage. Design for fast rollback:

```
Target rollback time from decision to complete:
  Feature flag rollback:     < 2 minutes  (best)
  Config change rollback:    < 5 minutes
  Redeployment rollback:     < 15 minutes
  Manual traffic shift:      < 5 minutes

If your current rollback takes > 15 minutes:
  → Add a feature flag or quick config toggle before next deployment
  → Slow rollback is itself a deployment risk
```
