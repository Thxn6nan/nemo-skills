# Prioritization Framework

Tools for deciding what to Kill, Keep, Grow, or Start.

---

## Part 1: Value-Effort Matrix

Plot each item on two dimensions before classifying it:

```
         HIGH VALUE
              │
   START/     │     GROW
   GROW ──────┼────────────→ LOW EFFORT
              │
   KILL  ──── │ ──── KEEP
              │
         LOW VALUE
```

### Quadrant Definitions

**HIGH VALUE + LOW EFFORT → GROW immediately**
These are the highest-leverage investments. They produce significant value
relative to the resources required. Prioritize these above all others.
If you have a GROW item and aren't investing in it, that is the primary waste.

**HIGH VALUE + HIGH EFFORT → START or GROW with planning**
Worth doing but requires deliberate resourcing. Don't start these until
you have resolved the higher-priority GROW items. Plan the approach
before committing — high effort means high cost if the direction is wrong.

**LOW VALUE + LOW EFFORT → KEEP or KILL based on drag**
If these are already done and running, keep them (low cost to maintain).
If these are on the roadmap and not yet started, skip them — there is
always something better to do with the effort.

**LOW VALUE + HIGH EFFORT → KILL**
These are the most dangerous items. They consume real resources for
minimal return. If currently in progress: cut losses and stop.
If planned: remove from roadmap. These items often persist because
of sunk cost thinking ("we've already put so much into it").

---

## Part 2: Kill Decision Rules

Kill is the hardest recommendation to make and the most valuable one to act on.
Use these rules to make Kill decisions defensible.

### Rule 1: The Honest Value Test
```
Ask: "If we removed this entirely tomorrow, what would actually be worse?"
  Nothing significant → KILL
  Someone would complain but nothing would break → KEEP (low cost)
  Real value would be lost → don't kill

The version of this question that reveals the truth:
"Would we build this if we were starting fresh today, knowing what we know?"
  NO → strong signal to kill
  MAYBE → investigate further
  YES → keep or grow
```

### Rule 2: The Compounding Test
```
Ask: "Does this get more valuable over time, or less?"
  Gets more valuable (data accumulates, users grow, knowledge compounds) → KEEP or GROW
  Stays the same value → KEEP if low maintenance cost
  Gets less valuable (world changes, usage declines, dependencies age) → consider KILL

A feature or component that requires constant maintenance to stay at the same
value is a candidate for killing when a better alternative exists.
```

### Rule 3: The Resource Drain Test
```
Tally the true cost of maintaining this item:
  - Engineering time per month to keep it running
  - Time spent on bugs/issues related to this
  - Cognitive load: how much does this add to the system's complexity?
  - Opportunity cost: what else could this resource do?

If total cost > value delivered → Kill candidate
If maintenance is near-zero → Keep (even low-value things are worth keeping if free)
```

### Rule 4: The Momentum Test
```
Has this item shown any positive momentum in the last 60-90 days?
  YES → don't kill yet; investigate what's working
  NO  → if it hasn't grown in 3 months despite reasonable effort, it probably won't

Exception: something in early stage may not have had time to show momentum.
Don't kill new things prematurely. Apply this rule to things that have
had sufficient time and resources to show results.
```

### Sunk Cost Override
```
The most common reason projects keep dead features alive:
"We've already put a lot of work into this."

Past investment is not a reason to continue future investment.
The question is never "what have we spent?" but "what will we get from here?"

When evaluating a Kill decision, explicitly ask:
"If we had not started this, and someone proposed starting it today,
would we approve it?"
  NO → the sunk cost is distorting judgment. Kill it.
```

---

## Part 3: Grow Decision Rules

Not everything with potential should be grown. Growing something requires
resources — and those resources come from somewhere.

### The Prerequisite Check
```
Before committing to GROW:
  □ Is the primary bottleneck (from potential analysis) resolved?
    If no → growing anything else is lower leverage than fixing the bottleneck

  □ Do we understand WHY this item is valuable?
    Vague value ("it's useful") → investigate before growing
    Clear value ("it reduces X by Y for Z users") → grow

  □ Is there a clear mechanism linking investment to outcome?
    "More resources → more output → more value" must be explicit
    If the link is unclear → validate before growing

  □ Is there a ceiling to this item's value?
    If the ceiling is narrow → grow efficiently, not expansively
    If the ceiling is high → growing deserves priority resourcing
```

### Grow vs Over-Invest
```
There is a point at which adding more resources to a GROW item
produces diminishing returns. Signs of over-investment:
  - Progress on this item is no longer limited by resources
  - Quality improvements are getting smaller despite same effort
  - The team working on this has more capacity than work
  - Adding people is making coordination harder, not faster

At this point: stabilize resources on this item and redirect to the next GROW.
```

---

## Part 4: The One Thing Rule

After completing Kill / Keep / Grow / Start classification, identify
**the single most important next action**.

### Finding The One Thing

```
Step 1: List all GROW and START items
Step 2: For each, ask: "What would happen to everything else if this succeeded?"
  → The item where success most unlocks other things → The One Thing

Step 3: Verify it addresses the primary bottleneck
  → If The One Thing doesn't address the primary bottleneck → reconsider

Step 4: Make it concrete
  NOT: "Improve the reliability of the pipeline"
  YES: "Add retry logic with exponential backoff to the data ingestion step
        so it handles the 429 errors that are causing 15% job failure rate"

The One Thing must be:
  □ Specific enough to know when it's done
  □ Achievable in the next 1-2 weeks (not a multi-month epic)
  □ Clearly linked to the highest-value outcome
  □ Assigned to someone (or this skill is recommending who should do it)
```

---

## Part 5: Direction Trade-off Matrix

When presenting strategic directions, use this format to make trade-offs explicit:

```
              DIRECTION A        DIRECTION B        DIRECTION C
              ─────────────      ─────────────      ─────────────
What grows:   <what improves>    <what improves>    <what improves>
What shrinks: <what's traded>    <what's traded>    <what's traded>
Time to see   <how long until    <how long until    <how long until
results:       results visible>   results visible>   results visible>
Risk:         <main risk>        <main risk>        <main risk>
Best if:      <conditions for    <conditions for    <conditions for
               choosing this>     choosing this>     choosing this>
```

Good strategic directions are **mutually exclusive** — choosing one makes
the others harder or impossible. If two directions don't trade off against
each other, they're not truly distinct directions.
