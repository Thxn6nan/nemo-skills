# Bias Correction Guide

Systematic corrections for the ways human judgment fails on goal evaluation.
Apply these in Phase 6 to stress-test the estimates from Phases 2–5.

---

## Bias 1: Optimism Bias — Value Inflation

**What it is:** People systematically overestimate the value of their goals
because they visualize the success scenario more vividly than the failure scenario.

**How to detect it:**
```
Ask: "What is the single most important assumption embedded in the value estimate?"
Then ask: "What is the probability that assumption is correct?"

Common inflated assumptions:
  ✗ "Users will adopt this as quickly as our last launch"
  ✗ "The team will stay focused on this for the full timeline"
  ✗ "This solves the problem completely, not partially"
  ✗ "No competing priorities will pull resources away"
  ✗ "The value will be visible within 3 months"
```

**How to correct it:**
```
Step 1: Write down the value estimate as calculated
Step 2: Ask "What does the pessimistic case look like — not catastrophic, 
         just the boring realistic bad outcome?"
Step 3: Calculate value in that scenario
Step 4: Take a weighted average:
         Corrected value = (optimistic × 0.25) + (realistic × 0.50) + (pessimistic × 0.25)

Example:
  Optimistic value:   $50k
  Realistic value:    $25k
  Pessimistic value:  $8k
  
  Corrected = (50k × 0.25) + (25k × 0.50) + (8k × 0.25)
            = 12.5k + 12.5k + 2k
            = $27k (vs the $50k originally estimated)
```

---

## Bias 2: Planning Fallacy — Cost Deflation

**What it is:** The systematic tendency to underestimate time and cost for
goals, even when there is abundant evidence of past overruns.
First documented by Kahneman & Tversky; replicated extensively in engineering.

**How to detect it:**
```
The tell: "This time it's different because..."
Common deflation patterns:
  ✗ Estimating only implementation time, not coordination, review, testing
  ✗ Assuming no interruptions or competing priorities
  ✗ Not accounting for the learning curve on unfamiliar parts
  ✗ Forgetting that similar tasks took longer than estimated before
  ✗ Planning for best-case sequences only
```

**How to correct it:**

*Method 1: Reference class forecasting*
```
Find the most comparable goal this team has completed before.
How long did it actually take vs. estimated?
Apply the same ratio to the current estimate.

Example:
  "The last API integration was estimated at 2 weeks, took 5 weeks"
  Current estimate: 3 weeks
  Correction factor: 5/2 = 2.5×
  Corrected estimate: 3 × 2.5 = 7.5 weeks
```

*Method 2: Complexity multiplier*
```
Assess the type of work, apply a standard multiplier to the estimate:

  Work type                              Multiplier
  Familiar, well-defined, solo           ×1.2
  Familiar domain, small team            ×1.5
  Moderate complexity, cross-team        ×2.0
  Novel problem, uncertain solution      ×2.5
  Strategic/organizational change        ×3.0+
```

*Method 3: Explicit hidden cost inventory*
```
List every category of work not in the original estimate:
  □ Stakeholder alignment meetings
  □ Code review and iteration cycles
  □ Testing (unit, integration, edge cases)
  □ Documentation
  □ Deployment and monitoring setup
  □ Bug fixes in the first 2 weeks post-launch
  □ Handoff to other teams
  □ Interruptions and context-switching
  
Add the estimated time for each. This typically adds 40–80% to initial estimates.
```

---

## Bias 3: Sunk Cost Fallacy — Past Investment as Justification

**What it is:** Using resources already spent as a reason to continue,
rather than evaluating whether future resources are worth spending.

**How to detect it:**
```
Warning phrases that signal sunk cost reasoning:
  ✗ "We've already put 3 months into this"
  ✗ "We can't abandon it now after all this work"
  ✗ "We're too far in to stop"
  ✗ "All that work would have been wasted"
```

**How to correct it:**
```
The Blank Slate Test:
  "Imagine we have not started this goal. Someone proposes it today.
   Given everything we now know (including what hasn't worked),
   would we approve starting it?"

  YES → proceed; past investment happens to align with good judgment
  NO  → the sunk cost is distorting judgment; past investment is gone
         either way; the question is only whether future investment is justified

The Partial Sunk Cost Question:
  "What has been built so far that is genuinely reusable even if we stop?"
  If the answer is "not much" → sunk cost argument is even weaker.
```

---

## Bias 4: Motivated Reasoning — Wanting the Answer

**What it is:** Evaluating in order to justify a conclusion already reached,
rather than to find the truth. Common when the evaluator proposed the goal
or has a personal stake in the outcome.

**How to detect it:**
```
Signs of motivated reasoning in an evaluation:
  ✗ Every piece of evidence is interpreted as supporting the goal
  ✗ Counterarguments are dismissed quickly without serious engagement
  ✗ The DROP verdict was never seriously considered
  ✗ Risk factors are noted but minimized ("yes but we'll handle it")
  ✗ The evaluation was built as a case, not as an inquiry
```

**How to correct it:**

*Method 1: Steel-man the opposing verdict*
```
Before finalizing, write the strongest possible case for the verdict
you are NOT recommending. Include:
  - The best arguments against pursuing this
  - The most likely ways it fails
  - What a skeptical senior person would say

Then ask: "Is our rebuttal to these arguments actually convincing?"
If the rebuttal is weak → reconsider the verdict.
```

*Method 2: Role separation*
```
Separate the role of advocate from the role of evaluator.
The person who proposed the goal should not be the sole evaluator.
If they must be, they should explicitly argue both sides.
```

---

## Bias 5: Scope Insensitivity — Treating All Sizes Equally

**What it is:** Emotional reaction to a goal doesn't scale proportionally
with its actual size or impact. A feature affecting 100 users and one affecting
10,000 users can generate the same enthusiasm, despite 100× difference in value.

**How to detect it:**
```
Ask: "If this affected 10× more people / generated 10× more value,
      would we prioritize it differently?"
  YES → the current scope is not being valued at the right level
  NO  → scope sensitivity is working correctly

Ask: "If this affected 10× fewer people, would we still do it?"
  YES → examine whether it's really worth it at the true scale
```

**How to correct it:**
```
Always state scope numerically during value estimation:
  NOT: "This helps our users with a frustrating problem"
  YES: "This affects the 200/1,000 users who experience X weekly,
        saving them approximately 30 minutes per occurrence"

Then ask: "Are 200 people × 30 minutes enough to justify this cost?"
The concrete numbers make scope insensitivity much harder to maintain.
```

---

## The Pre-Mortem Protocol

Run this in Phase 4 (Probability) and again in Phase 6 (Bias Correction).

**Purpose:** Force consideration of failure before committing, while there
is still time to address failure causes or adjust the decision.

**Protocol:**
```
Step 1 — Set the scene
  "It is [date 6 months from now]. This goal has failed.
   Not catastrophically — just didn't achieve what we intended.
   We are in a retrospective asking: what happened?"

Step 2 — Generate failure causes independently
  Each evaluator writes their top 3 reasons it failed.
  Do not share until everyone has written.

Step 3 — Collect and cluster
  Group similar causes. The ones that appear most frequently
  across evaluators are the most credible risks.

Step 4 — Assess each cause
  For each significant failure cause:
    □ How likely is this, given current conditions?
    □ Is this cause present right now?
    □ Can it be mitigated before starting?
    □ How much does this reduce our probability estimate?

Step 5 — Update the probability
  For each cause assessed as "present now" and "not mitigable":
    Reduce base probability by 10–20% per cause

Step 6 — Decide whether to address causes or adjust the verdict
  Causes that are addressable → add to prerequisites before pursuing
  Causes that are not addressable → reduce probability; may change verdict
```

**The pre-mortem is not optional for:**
- Goals with high reversal cost (irreversible commitments)
- Goals that failed before in a similar form
- Goals where the team is highly confident (confidence ≠ accuracy)
