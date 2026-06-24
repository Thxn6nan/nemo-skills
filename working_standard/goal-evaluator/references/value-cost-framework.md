# Value & Cost Framework

Methods for estimating value and cost honestly.

---

## Part 1: Value Estimation

### Clarity Questions (Phase 1 gate)

Before estimating any value, the goal must pass these:

```
1. "What is the one-sentence definition of success for this goal?"
   → If cannot be stated clearly: REFINE immediately

2. "How will we know when this goal is complete vs ongoing?"
   → If no clear completion signal: clarify or the goal will run forever

3. "What are we measuring to know if this worked?"
   → If no metric exists: define one before proceeding

4. "Who agrees this is the right goal?"
   → If stakeholders disagree: resolve before evaluating

5. "What assumption, if wrong, would invalidate this goal entirely?"
   → Surface the core bet before committing
```

---

### Direct Value Estimation

**Method 1: Time/Effort Saved**
```
Estimate for recurring time savings:
  Hours saved per instance × frequency per month × number of people × months

Example:
  2 hours saved × 20 instances/month × 5 people × 12 months
  = 2,400 person-hours/year
  
  Convert to cost: × average hourly cost → dollar value
  Convert to output: what else could those hours produce?
```

**Method 2: Error/Failure Reduction**
```
Value = (current failure rate − target failure rate)
        × frequency of occurrence
        × cost per failure

Example:
  Current failure rate: 8% | Target: 2%
  Frequency: 500 incidents/month
  Cost per failure: $200 (support time + customer impact)
  
  Monthly value = (0.08 − 0.02) × 500 × $200 = $6,000/month
```

**Method 3: Revenue/Conversion Impact**
```
Value = baseline revenue × expected lift % × confidence %

Example:
  Baseline monthly revenue: $50,000
  Expected lift: 15%
  Confidence in lift estimate: 60%
  
  Expected monthly value = $50,000 × 0.15 × 0.60 = $4,500/month
```

**Method 4: Qualitative Value — Scoring When Unquantifiable**
```
Use when value is real but not directly quantifiable (trust, morale,
capability, reputation). Score each dimension 1–5:

  Dimension             Weight   Score   Weighted
  User trust            30%      4       1.2
  Team capability       25%      3       0.75
  Strategic position    25%      4       1.0
  Competitive defense   20%      2       0.4
                                 ──────────────
  Qualitative score:             3.35 / 5.0

3.5+ → treat as high value for evaluation purposes
2.5–3.4 → moderate; compare against cost carefully
<2.5 → low qualitative value; quantitative value must carry the case
```

---

### Indirect Value: Unlocking Multiplier

Some goals are worth more than their direct value because they unlock other goals.

```
Identify dependencies:
  "What other goals does this enable that we couldn't do without it?"

  For each unlocked goal:
    - How valuable is that goal? (estimate)
    - How likely to be pursued? (probability)
    - How much does this goal accelerate it? (0-100%)

  Indirect value = Σ (value of unlocked goal × P(pursued) × acceleration%)

Example:
  This goal unlocks:
    Goal B: value $30k, 80% likely to be pursued, this enables 100% of it
    Goal C: value $20k, 50% likely, this contributes 60%
    
  Indirect value = (30k × 0.8 × 1.0) + (20k × 0.5 × 0.6)
                 = 24k + 6k = $30k indirect value
```

---

### Risk of NOT Pursuing (Inaction Cost)

Skipping a goal is not free. Estimate the cost of inaction:

```
Type 1: Competitive risk
  "If we don't do this, what does a competitor gain?"
  P(competitor acts) × cost of competitor advantage = inaction cost

Type 2: Technical debt accumulation
  "Does skipping this make the codebase harder to work with over time?"
  Monthly slowdown cost × months until forced to fix = inaction cost

Type 3: User trust erosion
  "Does skipping this erode user confidence or increase churn?"
  Estimated churn increase × LTV per user = inaction cost

Type 4: Window closing
  "Is there a specific window where this goal is possible that closes?"
  Value of goal × P(window closes in 6 months) = time-sensitive inaction cost
```

---

## Part 2: Cost Estimation

### The Three-Layer Cost Model

**Layer 1: Resource Cost (what most people estimate)**
```
People:
  Hours to complete × hourly cost (or opportunity value) per person
  Include: meetings, review, testing — not just implementation
  Apply planning fallacy multiplier:
    ×1.2 for familiar, well-scoped work
    ×1.5 for moderate complexity
    ×2.0 for novel or cross-team work

Money:
  Direct spend (tools, infrastructure, contractors)
  Ongoing operational cost (monthly, not just one-time)
  Include the 12-month total, not just the build cost
```

**Layer 2: Opportunity Cost (what most people miss)**
```
List the top 2-3 things that WON'T get done if this is pursued.
For each, estimate its value (even roughly).

The opportunity cost of this goal = sum of forgone value

If opportunity cost > direct value of this goal → reconsider

Rule of thumb: if you can't name what won't get done,
you haven't honestly assessed the opportunity cost.
```

**Layer 3: Reversal Cost (what determines how careful to be)**
```
Reversibility spectrum:
  Fully reversible  → stop and nothing is lost except time spent so far
  Mostly reversible → can undo most changes with moderate effort
  Partially reversible → some changes are permanent; some recoverable
  Irreversible      → cannot be undone; commits to a path

Cost if wrong:
  Fully reversible:   cost = time spent (low)
  Mostly reversible:  cost = time + recovery effort
  Partially rev.:     cost = time + recovery + permanent change effects
  Irreversible:       cost = time + all downstream consequences

Higher reversal cost → higher bar required before commitment.
Lower reversal cost → lower bar; move faster, learn faster.
```

---

## Part 3: Opportunity Cost Decision Matrix

```
                    HIGH OPP. COST          LOW OPP. COST
                  (strong alternatives)   (weak alternatives)
                 ┌──────────────────────┬──────────────────────┐
  HIGH VALUE     │  Compare carefully   │  PURSUE              │
  of this goal   │  EV math required    │  Clear winner        │
                 ├──────────────────────┼──────────────────────┤
  MODERATE VALUE │  Likely DROP or      │  DESCOPE or PURSUE   │
  of this goal   │  DEFER               │  depending on cost   │
                 ├──────────────────────┼──────────────────────┤
  LOW VALUE      │  DROP                │  DEFER or DROP       │
  of this goal   │                      │                      │
                 └──────────────────────┴──────────────────────┘
```

---

## Part 4: Scope Calibration

### Is the scope too small to bother?

A goal may have positive EV but still not be worth pursuing if:

```
The fixed overhead exceeds the net benefit:
  Every goal has overhead: coordination, context-switching, planning,
  communication, and the cognitive load of tracking it.
  
  Overhead threshold (rough estimate):
    Simple solo task:     4–8 hours overhead
    Team task:            1–2 days overhead
    Cross-team project:   1–2 weeks overhead

  If expected value < overhead cost → DESCOPE or DROP.
  The work itself may be correct but the packaging is wrong.

  Fix: bundle with another small goal, or eliminate the overhead
  by making it part of ongoing work rather than a standalone goal.
```

### Is the scope too large to succeed?

```
Goals that are too large suffer from:
  - Probability drops significantly with scope size
  - Reversibility decreases (more committed before learning)
  - Opportunity cost increases (blocks everything for longer)
  - Value is back-loaded (no signal until too late to stop)

Signal that scope is too large:
  □ Completion is measured in months, not weeks
  □ Success depends on multiple teams agreeing
  □ Value can only be measured after the whole thing is done
  □ "We'll figure out the details as we go"

Fix: DESCOPE to the minimum version that validates the core assumption.
Run that first. Then decide whether to expand.
```
