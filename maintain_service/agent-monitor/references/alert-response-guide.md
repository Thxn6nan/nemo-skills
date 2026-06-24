# Alert Response Guide — Playbooks by Alert Type

For each alert type: what it means, how to confirm it's real,
what to investigate, and what actions to take.

---

## R1: Quality Score Drop

**Alert fires when:** Weekly eval score drops >10pp from baseline

```
STEP 1 — Confirm it's real (5 minutes)
  □ Is the eval set the same one used for baseline? (not accidentally changed)
  □ Is the scoring rubric unchanged?
  □ Are there obvious data pipeline issues (eval jobs errored, partial run)?
  If any → fix the monitoring first, re-run eval before investigating agent

STEP 2 — Identify when it started (10 minutes)
  □ Is this a sudden drop or a gradual decline?
    Sudden: likely caused by a specific change (check deploy log)
    Gradual: likely caused by data/input drift or model drift

  □ Pull last 7 days of daily quality samples — when did score change?

STEP 3 — Narrow the cause (20 minutes)
  □ Has there been a deployment in the relevant window?
    YES → likely deployment regression; review what changed
    NO  → look at input drift or external factors

  □ Is quality dropping across all task types, or one specific type?
    All types → systemic issue (prompt, model, tools)
    One type → that category's inputs or handling changed

  □ Pull 10 low-quality samples — what do they have in common?

STEP 4 — Action
  Deployment regression identified → run agent-deploy rollback
  Specific task type degraded → run agent-audit on that category
  Input drift (users sending new types of queries) → run project-compass
  No cause found after 1 hour → escalate to PAGE; consider rollback
```

---

## R2: Hallucination Rate Spike

**Alert fires when:** Hallucination rate rises >5pp from baseline

```
STEP 1 — Confirm and scope (10 minutes)
  □ Which hallucination type(s) are spiking? (H1/H2/H3/H4/H5/H6)
  □ Is this from a specific document type or query category?
  □ Are the inputs to the agent different from normal? (new data source, etc.)

STEP 2 — Immediate risk assessment
  □ Are any irreversible actions (send, delete, charge) being taken
    based on outputs that may be hallucinated?
    YES → halt those actions immediately pending investigation
    NO  → continue investigation without immediate halt

STEP 3 — Narrow the cause
  H1 (Numerical): Did a calculation tool break or get removed?
  H3 (Data Drift): Did source documents change? Is RAG retrieval degraded?
  H5 (Citation): Did the agent lose access to its source documents?
  All types rising: Was there a model version change?

STEP 4 — Action
  Tool failure causing H1 → fix tool; verify with anti-hallucination
  RAG degraded causing H3 → run anti-hallucination PREVENT mode; rebuild index
  Model change causing all → evaluate if new model needs prompt adjustment
  Cannot identify cause → run anti-hallucination full VERIFY mode on outputs;
                          consider rollback if rate is >15%
```

---

## R3: Cost Spike

**Alert fires when:** Cost per task rises >50% from baseline

```
STEP 1 — Confirm and quantify (5 minutes)
  □ Is this real cost or a monitoring/attribution error?
  □ What is the actual dollar impact? (cost spike × volume = $X/day)
  □ Is volume also up? (more tasks at same cost ≠ cost spike)

STEP 2 — Identify the driver (15 minutes)
  □ Check token usage breakdown: input tokens or output tokens spiking?
    Input tokens spiking:
      → Context window growing? Agent accumulating unnecessary context
      → Source documents in RAG getting larger?
      → System prompt changed and grew?
    Output tokens spiking:
      → Agent generating much longer responses than baseline
      → Retry loops? (agent retrying tool calls, generating multiple attempts)

  □ Check tool call rate: has it increased?
    YES → agent is making more calls per task (retry loops or inefficiency)
    NO  → cost is from token usage alone

  □ Was there a model version change?
    YES → may have switched to a more expensive tier unintentionally

STEP 3 — Action
  Retry loop identified → fix tool that's causing retries; add circuit breaker
  Context growing → add context pruning; review agent-engineering Efficiency
  Output growing → add output length constraints to prompt
  Model tier change → verify intended; update cost baseline if intentional
  Unknown cause → run agent-engineering Efficiency review
```

---

## R4: Completion Rate Drop

**Alert fires when:** Task completion rate drops >8pp from baseline

```
STEP 1 — What does "not completed" look like? (10 minutes)
  □ Agent erroring out (tool failures, timeouts)?
  □ Agent escalating to human more than before?
  □ Agent producing output but user re-submitting (outcome failure)?
  □ Agent refusing requests it previously handled?

STEP 2 — Narrow by task type
  □ Is every task type affected, or specific ones?
  □ Is there a time pattern? (started at specific hour or after specific event)

STEP 3 — Investigate by failure type
  Tool errors causing incompletions:
    → Check tool availability and error rates
    → Add retry logic if not present; verify tools haven't changed API

  Escalations increasing:
    → Are users sending harder queries than before?
    → Did escalation threshold in prompt change?
    → Run agent-qa to check if scope narrowed unintentionally

  Output produced but user re-submits:
    → Quality issue, not completion issue
    → Re-classify as R1 (Quality Score Drop) and investigate there

  Agent refusing:
    → Prompt may have become overly restrictive
    → Check if prompt was recently changed

STEP 4 — Action
  Tool failure → fix tool; add monitoring for that specific tool
  Scope narrowed unintentionally → review recent prompt changes; revert if needed
  Input drift → run project-compass; user needs may have evolved
```

---

## R5: Scope Drift Alert

**Alert fires when:** agent-qa POST sampling returns DRIFTED or RUNAWAY scores

```
STEP 1 — Review the specific samples (15 minutes)
  □ What unauthorized actions did the agent take?
  □ Is this consistent across samples or isolated cases?
  □ Did scope drift cause any irreversible actions?

STEP 2 — Identify cause
  □ Is the agent "helpful" in ways that weren't asked for?
    → Prompt may lack explicit prohibitions
  □ Is the agent interpreting ambiguous requests too broadly?
    → Need clearer scope boundaries in prompt
  □ Did a specific type of input trigger the drift?
    → Add handling for that input type

STEP 3 — Action
  Isolated cases → document; add to test set; monitor
  Consistent pattern → update prompt scope boundaries; re-deploy
  Irreversible actions taken → run agent-deploy rollback; full audit
  Systemic → run agent-qa PRE to redefine scope; re-deploy with tighter prompt
```

---

## Monthly Drift Review Template

Run this every month to catch slow-building issues that don't trigger alerts:

```
MONTHLY DRIFT REVIEW — <Month Year>
════════════════════════════════════════

METRIC TRENDS (this month vs. last 3 months):
  Quality score:       <current> vs <3-month avg> — [STABLE|UP|DOWN]
  Hallucination rate:  <current> vs <3-month avg> — [STABLE|UP|DOWN]
  Cost per task:       <current> vs <3-month avg> — [STABLE|UP|DOWN]
  Completion rate:     <current> vs <3-month avg> — [STABLE|UP|DOWN]
  Scope drift:         <current> vs <3-month avg> — [STABLE|UP|DOWN]

INPUT DRIFT CHECK:
  Are users asking different types of questions than 3 months ago?
  Has the distribution of task types shifted?

FINDINGS:
  □ All stable → no action; continue current cadence
  □ Slow upward trend in [metric] → investigate before it becomes an alert
  □ Baseline should be updated → run agent-deploy baseline update
  □ Direction change may be needed → trigger project-compass

RECOMMENDED ACTIONS:
  1. <action>
  2. <action>

REVIEWED BY: <name>  DATE: <date>
════════════════════════════════════════
```
