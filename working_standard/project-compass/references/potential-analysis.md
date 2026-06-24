# Potential Analysis Reference

Tools for assessing trajectory, ceiling, and bottlenecks.

---

## Part 1: Trajectory Signals

Momentum is determined by looking at the **direction of change**, not just the
current state. A project scoring 4/10 that was 2/10 last month has better
momentum than a project at 7/10 that was 9/10 last month.

### Positive Momentum Signals
```
Usage / adoption signals:
  ✦ User count, session count, or engagement growing week-over-week
  ✦ Organic usage (people finding and using it without being told)
  ✦ Users returning (retention > one-time use)
  ✦ Users doing more than the core use case (expansion)

Quality signals:
  ✦ Bug reports decreasing relative to usage
  ✦ Build / deploy cycles getting faster
  ✦ Team velocity increasing (same effort → more output)
  ✦ External positive feedback increasing

Strategic signals:
  ✦ Key dependencies (people, tools, data) becoming more available
  ✦ Market or context moving in the project's direction
  ✦ Team knowledge compounding (each problem solved makes next one easier)
```

### Negative Momentum Signals
```
Usage / adoption signals:
  ✦ Flat or declining active use despite investment
  ✦ High churn (users try once, don't return)
  ✦ Usage concentrated in 1-2 power users, not spreading
  ✦ Core use case not used as imagined

Quality signals:
  ✦ Same bugs recurring or re-appearing
  ✦ Technical debt slowing each new addition
  ✦ More time spent maintaining than building
  ✦ Workarounds accumulating ("we'll fix it later")

Strategic signals:
  ✦ Key assumptions underlying the project have proven wrong
  ✦ External context has shifted against the project
  ✦ Team energy / belief declining
  ✦ Resources being diverted away
```

### Stall Signals (different from decline)
```
A stall is when the project is neither growing nor shrinking — just stuck.
Signs of a stall:
  ✦ Metrics flat for 4+ weeks despite consistent effort
  ✦ The "next big thing" keeps getting delayed without clear reason
  ✦ Conversations about the project loop back to the same topics
  ✦ Team is busy but output isn't moving the key metrics
  ✦ No clear answer to "what would double our progress?"

Stalls often have a hidden root cause that isn't being addressed.
Look for the assumption that everyone is avoiding examining.
```

---

## Part 2: Potential Ceiling Analysis

The ceiling is the **best realistic outcome** — not the dream case, not the
average case. It answers: if this project executed well on a good path, what
could it actually become?

### Ceiling Factors

**Ceiling-raising factors** (each raises the potential outcome):
```
  ↑ Large addressable problem with few existing solutions
  ↑ Strong moat: data, network effects, or hard-to-replicate capability
  ↑ Compounding dynamics: value grows with usage/time/scale
  ↑ Multiple use cases unlocked by the same core capability
  ↑ Strategic position: this becomes more valuable as adjacent things grow
  ↑ Team has rare or hard-to-acquire knowledge in this domain
```

**Ceiling-limiting factors** (each lowers the potential outcome):
```
  ↓ Narrow addressable problem (niche with inherent size limit)
  ↓ Easy to copy once it works (no durable advantage)
  ↓ Diminishing returns: value plateaus as usage grows
  ↓ Single use case with no natural expansion path
  ↓ Dependent on an external platform that could change the rules
  ↓ Core insight requires constant human judgment to maintain
```

### Ceiling Estimation Framework

```
Step 1 — Define what "10x better" looks like
  If this project succeeds fully, what does the user/business achieve?
  Be concrete: not "more efficient" but "cuts X from 2 hours to 10 minutes"

Step 2 — Identify who and how many benefit
  Who has this problem? How many of them are there?
  Is the problem recurring (subscription value) or one-time (transaction value)?

Step 3 — Identify what would have to be true
  List 3-5 conditions that must hold for the ceiling to be reached.
  If any seem unlikely → note as a ceiling constraint.

Step 4 — Reality-check against what you've seen so far
  Does current evidence support the ceiling estimate?
  If the ceiling is "high" but early signals are weak → investigate the gap.
```

### Ceiling Ratings

```
EXCEPTIONAL: Compounding, expanding, defensible value. Rare.
  → Invest heavily; the ceiling justifies significant resource commitment

HIGH: Large problem, clear path, meaningful advantages.
  → Prioritize; worth sustained investment

MODERATE: Real value but bounded. Limited expansion potential.
  → Efficient execution matters more than growth investment

NARROW: Solves a specific problem well, inherently limited scope.
  → Optimize for efficiency and cost; don't over-invest in growth
```

---

## Part 3: Bottleneck Identification

Based on Theory of Constraints: in any system, one constraint limits throughput
more than all others combined. Improving anything else doesn't improve the system.

### The Five Bottleneck Types

**Technical bottleneck** — the code/system can't do what's needed
```
Signals:
  - Features blocked because the platform doesn't support them
  - Performance too slow for the intended use case
  - Integration missing that everything else depends on
  - Architecture requires rewrite before moving forward

Test: "If we had unlimited engineers, would this unblock us?"
  YES → Technical bottleneck
  NO  → Look at other types
```

**Knowledge bottleneck** — the team doesn't know what they need to know
```
Signals:
  - Decisions keep being deferred because of uncertainty
  - Same questions raised without being answered
  - Experiments not being run that would answer key questions
  - Team disagrees on fundamentals without evidence to resolve it

Test: "If we knew the answer to [X], would we move faster?"
  YES → Knowledge bottleneck — define and run the experiment
  NO  → Look at other types
```

**Resource bottleneck** — not enough time, money, or people
```
Signals:
  - Roadmap items clearly valuable but constantly deprioritized
  - Right things identified but no one to execute them
  - Progress proportional to resource input (more resource = more progress)

Test: "If we had 2x resources applied to this, would it move?"
  YES → Resource bottleneck
  NO  → Adding resources won't help — look at other types
```

**Direction bottleneck** — unclear or wrong direction
```
Signals:
  - Team rebuilds or rethinks things frequently
  - Disagreement about what to build next
  - Features built but not used as expected
  - "We need to figure out the strategy" said repeatedly

Test: "If we had clear direction, would execution follow naturally?"
  YES → Direction bottleneck — this skill is most useful here
  NO  → Direction is not the problem
```

**External bottleneck** — something outside the team's control
```
Signals:
  - Waiting on approvals, partners, data, or decisions from others
  - Dependencies on external systems not yet available
  - Regulatory or market timing not under team control

Test: "Is the blocker something we can influence or must we wait?"
  WAIT → External bottleneck — plan around it or find a path that doesn't depend on it
  INFLUENCE → It's not truly external — look at other types
```

### Finding the Primary Bottleneck

```
The primary bottleneck has these properties:
  1. Resolving it unlocks progress that other changes cannot
  2. It explains why effort isn't translating to outcome
  3. The team may be unconsciously avoiding it (because it's hard)

Warning: The stated bottleneck is often not the real one.
  Stated: "We need more engineers"
  Real:   The direction isn't clear enough to use more engineers well

Always ask: "If we resolved this, would we actually move faster? Or would
the next bottleneck immediately take its place?" If the answer is "the next
bottleneck takes over immediately" → the stated one is not primary.
```
