# Deploy Checklist — Pre-Flight by Environment

Run the appropriate checklist before every deployment.
All items must be ✅ before proceeding. No exceptions without documented waiver.

---

## Checklist L1: Staging Deployment

```
QUALITY
□ agent-audit run on new version — no Critical or High defects
□ anti-hallucination check run if agent handles data (trust rating ≥ MODERATE)
□ Eval set results: quality metric ≥ current staging baseline
□ Manual review of 10 output samples — no unexpected behavior

SAFETY
□ Rollback procedure written out step-by-step
□ Rollback tested in dev environment at least once
□ All irreversible tool actions identified
□ Irreversible actions have a confirmation gate or dry-run mode

SCOPE
□ agent-qa PRE mode approved: scope of change documented
□ Prompt diff reviewed line-by-line (no accidental changes)
□ Tool versions pinned (no silent upstream changes)
□ Environment variables confirmed correct for staging

OPERATIONAL
□ Monitoring configured to capture new version's outputs
□ Staging eval set is realistic (not just toy examples)
□ External service mocks or staging equivalents connected
□ Test includes at least 1 adversarial input
```

---

## Checklist L2: Production Deployment (Canary/Gradual)

*Run after L1 staging checklist passes.*

```
QUALITY GATES
□ Staging eval results documented and approved
□ Quality delta vs current production: within tolerance
□ No regressions on previously-failing test cases that were fixed

SAFETY GATES
□ Rollback plan tested in staging (not just written)
□ On-call person confirmed available for deployment window
□ Maximum blast radius documented: "if this fails, X users are affected"
□ Irreversible actions blocked during canary phase if possible

ROLLOUT CONFIGURATION
□ Canary % set correctly in feature flag / load balancer
□ Monitoring alert thresholds confirmed (from agent-monitor)
□ Stage gate criteria documented: what must be true to expand to next %
□ Rollback trigger criteria documented: what will cause immediate rollback

COMMUNICATION
□ Stakeholders notified of deployment window
□ Known risks communicated: "watch for X in the first 24 hours"
□ Support team briefed if user-facing changes are included

OPERATIONAL
□ Deployment window chosen: avoid peak traffic hours
□ Team has capacity to monitor for the first 2 hours post-deploy
□ Logging confirmed: new version's outputs are being captured
□ Cost monitoring active: alert if cost/task increases >50%
```

---

## Checklist L3: Full Production Cutover

*Only for Low Risk deployments after canary/gradual completed successfully.*

```
□ All canary/gradual stages completed without rollback
□ Quality metrics stable across full rollout period
□ No pending incidents or unusual patterns
□ Baseline metrics updated to reflect new version's performance
□ Deployment record filed: version, changes, metrics, sign-off
□ agent-monitor thresholds updated if performance profile changed
```

---

## Irreversible Action Gate

For any agent that can take irreversible actions (send email, delete data,
post to external service, charge a user, etc.):

```
BEFORE DEPLOYING AGENTS WITH IRREVERSIBLE ACTIONS:

□ Dry-run mode exists and has been tested
  → Agent can run through its logic without executing the irreversible step
  → Dry-run output reviewed before live run is approved

□ Human-in-the-loop gate for first N executions
  → New version requires human approval for irreversible action for 48 hours
  → Only automated after N successful approved executions

□ Rate limit on irreversible actions
  → Agent cannot take more than X irreversible actions per hour
  → Spike above limit triggers alert + pause

□ Audit log of every irreversible action
  → Who/what triggered it, when, what was done, what the input was
  → Retained for minimum 30 days

□ Undo or compensating action documented
  → Even "irreversible" actions often have compensating actions
  → "Sent wrong email" → send correction email
  → "Deleted wrong record" → restore from backup
  → Document the compensating action before deploying
```

---

## Post-Deploy Sign-Off Template

```
DEPLOYMENT SIGN-OFF
═══════════════════════════════════════
Agent:          <name + version>
Deployed:       <timestamp>
Strategy used:  <shadow/canary/gradual/full>
Deployed by:    <name>

PRE-DEPLOY METRICS (baseline):
  Quality score:    <X>
  Error rate:       <X>%
  Cost per task:    <$X>
  Hallucination rate: <X>%

POST-DEPLOY METRICS (T+24h):
  Quality score:    <X>   [▲/▼/= vs baseline]
  Error rate:       <X>%  [▲/▼/= vs baseline]
  Cost per task:    <$X>  [▲/▼/= vs baseline]
  Hallucination rate: <X>% [▲/▼/= vs baseline]

RESULT: [SUCCESS | PARTIAL SUCCESS | ROLLED BACK]
SIGN-OFF: <name>
BASELINE UPDATED: [YES | NO — reason]
═══════════════════════════════════════
```
