# Metrics Guide — Agent-Specific Metrics and Thresholds

---

## Layer 1: Infrastructure Metrics

Standard. Most teams already track these. Listed for completeness.

```
METRIC          DESCRIPTION                  TYPICAL THRESHOLD
──────────────  ───────────────────────────  ─────────────────────────
Error rate      % of requests returning      WATCH >2× baseline
                non-200 or timeout           ALERT >5× baseline
                                             PAGE  >10× baseline

P95 Latency     95th percentile response     WATCH >1.5× baseline
                time                         ALERT >2× baseline
                                             PAGE  >5× baseline (SLA breach)

Throughput      Requests per minute          WATCH ±50% from normal
                                             ALERT ±80% from normal
                (spike AND drop matter)
```

---

## Layer 2: Quality Metrics

**These are the most important agent metrics. Most teams don't track them.**

### Task Completion Rate

```
DEFINITION:  % of tasks where the agent fully completed the requested work
             (vs. abandoned, errored, or escalated to human)

HOW TO MEASURE:
  Method A: Outcome labels (best)
    Each task is labeled: completed / partial / failed / escalated
    Sample 5-10% of tasks; label with a rubric

  Method B: Proxy metric (easier)
    Escalation rate as proxy for failure rate
    Tool call completion rate (did final tool succeed?)
    
THRESHOLD:
  WATCH:  drops >3pp from baseline
  ALERT:  drops >8pp from baseline
  PAGE:   drops >15pp from baseline

WHY IT MATTERS:
  A completion rate drop means users are getting less value.
  Often the first signal of prompt drift or tool failure.
```

### Quality Score

```
DEFINITION:  Rubric-based score of output quality on sampled outputs
             Typically 1-5 scale on: correctness, completeness, format

HOW TO MEASURE:
  Run a fixed eval set weekly (same 50-100 examples each time).
  Score each output on the rubric. Average is the quality score.
  Compare to baseline score from agent-deploy sign-off.

THRESHOLD (as delta from baseline):
  WATCH:  -5 percentage points
  ALERT:  -10 percentage points
  PAGE:   -20 percentage points

RUBRIC EXAMPLE (adapt to your agent's task):
  Correctness:   Is the output factually accurate?   (1-5)
  Completeness:  Does it fully address the request?  (1-5)
  Format:        Is it in the expected format?       (1-5)
  Tone/safety:   Is it appropriate for the context?  (1-5)
  Score = average of dimensions weighted by importance
```

### User Outcome Rate

```
DEFINITION:  % of users who achieved their goal after interacting with the agent
             (vs. needed to retry, escalate, or abandon)

HOW TO MEASURE:
  Direct:  Post-interaction rating or survey (if available)
  Proxy:   Did the user return with the same query within 24h?
           → Indicates the first response didn't solve the problem

THRESHOLD:
  WATCH:  return-with-same-query rate increases >5pp
  ALERT:  return-with-same-query rate increases >10pp
```

---

## Layer 3: Trust Metrics

### Hallucination Rate

```
DEFINITION:  % of sampled outputs containing at least one hallucinated claim
             (fabricated fact, wrong number, invented citation, data drift)

HOW TO MEASURE:
  1. Sample 5% of outputs that contain: numbers, facts, citations, summaries
  2. Run anti-hallucination verification protocol on each sample
  3. Hallucination rate = outputs with ≥1 contradiction / total sampled

THRESHOLD:
  WATCH:  +2pp above baseline
  ALERT:  +5pp above baseline
  PAGE:   +10pp above baseline OR any hallucination in irreversible-action context

SUBCATEGORIES TO TRACK SEPARATELY (if data quality allows):
  H1 Numerical:   wrong calculations (most impactful in financial/analytics agents)
  H3 Data Drift:  summaries that misrepresent source (most common in RAG agents)
  H5 Citation:    fabricated sources (most impactful in research agents)
```

### Source Grounding Rate

```
DEFINITION:  % of factual claims in outputs that can be traced to a source
             (relevant for RAG agents and document-processing agents)

HOW TO MEASURE:
  Sample 10 outputs per week.
  Count claims. Count claims with verifiable source attribution.
  Grounding rate = attributed / total claims.

THRESHOLD:
  WATCH:  drops >10pp from baseline
  ALERT:  drops >20pp from baseline
```

---

## Layer 4: Behavior Metrics

### Scope Drift Score

```
DEFINITION:  % of agent actions that were outside the task scope
             (from agent-qa POST mode)

HOW TO MEASURE:
  Sample 2% of task outputs monthly.
  Run agent-qa POST mode on each.
  Record the Scope Drift Score from each review.
  Average is the monitored metric.

THRESHOLD:
  WATCH:  average score >15% (LOOSE rating)
  ALERT:  average score >30% (DRIFTED rating on multiple samples)
  PAGE:   any RUNAWAY rating (>50%) in production
```

### Tool Call Rate

```
DEFINITION:  Average number of tool calls per task completion

HOW TO MEASURE:
  Log every tool call with task ID.
  Average calls per completed task.

WHY IT MATTERS:
  Rising tool call rate → agent is less efficient (Efficiency metric)
  Specific tool spiking → that tool may be failing silently (causing retries)
  Tool call rate dropping → agent may be answering from memory instead of tools

THRESHOLD:
  WATCH:  ±30% from baseline
  ALERT:  ±50% from baseline (either direction)
```

### Decision Consistency

```
DEFINITION:  Given the same input twice, does the agent give the same output?
             (measured as semantic similarity, not exact match)

HOW TO MEASURE:
  Monthly: run 10 identical test cases twice.
  Score semantic similarity of paired outputs (0-1).
  Average consistency score.

THRESHOLD:
  WATCH:  average consistency drops below 0.85
  ALERT:  average consistency drops below 0.70
```

---

## Cost Metrics

### Cost Per Task

```
DEFINITION:  Total API cost (input + output tokens × price) per completed task

HOW TO MEASURE:
  Sum all token costs for a period.
  Divide by number of completed tasks.

THRESHOLD:
  WATCH:  +20% above baseline
  ALERT:  +50% above baseline
  PAGE:   +100% above baseline (cost has doubled)

COMMON CAUSES OF COST SPIKE:
  - Context window growing (agent accumulating unnecessary context)
  - Retry loops (agent retrying failed tool calls repeatedly)
  - Model version change (different pricing tier)
  - Task complexity increase (users sending harder inputs)
```

### Cost Trend

```
DEFINITION:  Is cost per task trending up, down, or flat over time?

HOW TO MEASURE:
  Plot weekly average cost per task.
  Calculate week-over-week % change.
  7-day moving average to smooth noise.

THRESHOLD:
  WATCH:  +5% week-over-week for 2 consecutive weeks
  ALERT:  +10% week-over-week (accelerating cost growth)
```

---

## Metric Priority by Agent Type

Not every agent needs every metric equally. Focus on what matters most:

```
AGENT TYPE           TOP 3 METRICS TO PRIORITIZE
───────────────────  ────────────────────────────────────────────
Customer support     Completion rate, User outcome rate, Escalation rate
Data analysis        Hallucination rate (H1+H3), Quality score, Source grounding
Research/RAG         Hallucination rate (H5), Source grounding, Quality score
Code generation      Quality score, Scope drift, Tool call completion rate
Automation/action    Completion rate, Scope drift, Irreversible action error rate
General assistant    Quality score, Completion rate, Cost per task
```
