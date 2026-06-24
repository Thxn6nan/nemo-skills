# Direction Report Format

---

## Full Compass Report Template

```
╔══════════════════════════════════════════════════════════════╗
║              PROJECT COMPASS REPORT                          ║
╚══════════════════════════════════════════════════════════════╝

PROJECT:   <name or description>
CONTEXT:   <brief summary of current state, key constraints>
DATE:      <ISO date>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TRAJECTORY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Momentum:  [Building | Stable | Declining | Stalled]

Positive signals:
  + <what is growing or working without intervention>
  + <what is showing natural momentum>

Negative signals:
  - <what is slowing or degrading>
  - <what requires more effort for same output>

If nothing changes in 3 months:
  <concrete prediction — be honest, not optimistic>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
POTENTIAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Ceiling:  [Narrow | Moderate | High | Exceptional]

What this could realistically become:
  <concrete description of best-case outcome>

What must be true to reach the ceiling:
  1. <condition>
  2. <condition>
  3. <condition>

Current distance from ceiling:  [Close | Moderate | Far]
Primary gap type:  [Capability | Resources | Direction | Execution | Timing]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BOTTLENECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PRIMARY CONSTRAINT: <name it clearly>
  Type:     [Technical | Knowledge | Resource | Direction | External]
  Evidence: <why this is the constraint — what's stuck because of it>
  Effect:   <what becomes possible once this is resolved>

SECONDARY CONSTRAINTS: (will matter after primary is resolved)
  1. <constraint> — will surface when <condition>
  2. <constraint>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KILL / KEEP / GROW / START
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔴 KILL — Stop these now
  • <item>
    Why: <reason>
    Resource freed: <what is recovered by stopping this>

🟡 KEEP — Maintain without adding resources
  • <item>
    Why: <what value it provides, why it doesn't need more investment>

🟢 GROW — Invest more here
  • <item>
    Why: <what unlocks when this grows>
    What growth looks like: <concrete description of more investment>

🔵 START — Begin this; doesn't exist yet
  • <item>
    Why now: <why this is the right time>
    Prerequisite: <what must be true before starting>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STRATEGIC DIRECTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DIRECTION A: <name>
  Strategy:    <what this means in practice>
  What grows:  <what improves with this choice>
  Trade-off:   <what is sacrificed or deprioritized>
  Best if:     <conditions under which this is the right call>
  First move:  <most important first action>

DIRECTION B: <name>
  Strategy:    <what this means in practice>
  What grows:  <what improves with this choice>
  Trade-off:   <what is sacrificed or deprioritized>
  Best if:     <conditions under which this is the right call>
  First move:  <most important first action>

DIRECTION C: <name>  [optional — include if genuinely distinct from A and B]
  Strategy:    ...
  What grows:  ...
  Trade-off:   ...
  Best if:     ...
  First move:  ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RECOMMENDATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

RECOMMENDED DIRECTION: <A | B | C>
  Because: <specific reasoning for this project's situation>
  This wins over the alternatives because:
    vs Direction <X>: <why this is better given current context>
    vs Direction <Y>: <why this is better given current context>
  Assumption this depends on: <what must be true for this to be right>
  If that assumption is wrong: → reconsider Direction <other>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
THE ONE THING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

The single highest-leverage action right now:

  ▶ <specific, concrete, actionable — completable in 1-2 weeks>

  Why this one:
    <why this unlocks more than anything else on the list>

  How to know it's done:
    <observable success condition>

  When to revisit direction:
    <what signal or milestone should trigger a strategy review>

╔══════════════════════════════════════════════════════════════╗
║  MOMENTUM: [Building|Stable|Declining|Stalled]               ║
║  CEILING:  [Narrow|Moderate|High|Exceptional]                ║
║  DIRECTION: <A|B|C — name>                                   ║
╚══════════════════════════════════════════════════════════════╝
```

---

## Worked Example — Agent Pipeline Project

```
╔══════════════════════════════════════════════════════════════╗
║              PROJECT COMPASS REPORT                          ║
╚══════════════════════════════════════════════════════════════╝

PROJECT:   Multi-agent customer support pipeline
CONTEXT:   3 months in. Working prototype. Team of 2. Used by 1 internal team.
           Has: routing agent, resolution agent, escalation logic.
           Problem: resolution rate 40% (target: 70%), team losing confidence.
DATE:      2025-08-14

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TRAJECTORY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Momentum:  Declining

Positive signals:
  + Routing agent works well — correctly classifies 92% of tickets
  + Escalation logic catches the cases that would cause user harm
  + The internal team trusts it enough to use it daily

Negative signals:
  - Resolution rate stuck at 40% for 6 weeks despite iterations
  - Resolution agent frequently hallucinating policy details
  - Each new ticket category requires manual prompt engineering

If nothing changes in 3 months:
  Resolution rate stays at 40-45%. Internal team stops using it
  for complex tickets, reverting to manual handling. Project value
  narrows to "routing tool" only, which is a much lower ceiling.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
POTENTIAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Ceiling:  High

What this could realistically become:
  A system that resolves 70%+ of tier-1 tickets autonomously,
  with human review only on escalations. Frees 60% of support
  team time. Extensible to new ticket categories without manual work.

What must be true to reach the ceiling:
  1. Resolution agent grounded in verified policy documents (not memory)
  2. Feedback loop capturing why resolutions fail, fed back to improve
  3. Coverage extended to 3-4 ticket categories (currently: 1)

Current distance from ceiling:  Moderate
Primary gap type:  Capability (resolution quality)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BOTTLENECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PRIMARY CONSTRAINT: Resolution agent hallucinating policy details
  Type:     Technical + Knowledge
  Evidence: 35% of failed resolutions cite policies incorrectly or
            confidently state policy that doesn't exist. Agent is
            answering from training data, not from actual policy docs.
  Effect:   Once resolved, resolution rate should reach 60-65%.
            Remaining gap (to 70%) addressable with better prompting.

SECONDARY CONSTRAINTS:
  1. No feedback loop — failures aren't captured systematically,
     so the same mistakes repeat without learning
  2. Manual prompt work per category — will limit scaling to new
     ticket types; matters after primary constraint is resolved

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KILL / KEEP / GROW / START
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔴 KILL
  • Manual prompt engineering per ticket category
    Why: Doesn't scale. Time spent here is borrowed from
         fixing the root cause (grounding). Kill now.
    Resource freed: ~30% of current engineering time

🟡 KEEP
  • Routing agent (as-is)
    Why: Works at 92% accuracy; no investment needed right now

  • Escalation logic (as-is)
    Why: Safety-critical; working; don't touch while other things change

🟢 GROW
  • Resolution agent — specifically: grounding in policy documents
    Why: This is the primary bottleneck. Growing this directly
         addresses the 30-point gap between current and target

🔵 START
  • Failure capture system: log why each resolution failed
    Why now: Without this, improvements are guesswork.
             Every sprint without it is wasted learning.
    Prerequisite: Takes 1-2 days; do this before next sprint

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STRATEGIC DIRECTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DIRECTION A: "Fix the Floor" (Ground before Expand)
  Strategy:    Fix resolution quality in current category first.
               Reach 70% before expanding to new ticket types.
  What grows:  Resolution accuracy, user trust, team confidence
  Trade-off:   No new categories for 4-6 weeks
  Best if:     Team morale needs a win; current users are losing confidence
  First move:  Add RAG grounding to resolution agent using policy docs

DIRECTION B: "Parallel Build" (Fix + Expand simultaneously)
  Strategy:    One person fixes grounding; one person builds
               failure capture + second category in parallel.
  What grows:  Coverage and quality at the same time
  Trade-off:   Higher risk — two things changing at once makes
               it harder to attribute improvements
  Best if:     Team has clear capacity split and high coordination
  First move:  Assign explicit ownership: person A = grounding, person B = logging

DIRECTION C: "Narrow and Perfect" (Routing Only, for now)
  Strategy:    Accept current resolution limits. Double down on
               routing quality and handoff to human. Reframe
               as "intelligent triage" not "autonomous resolution."
  What grows:  Routing accuracy, handoff quality, adoption breadth
  Trade-off:   Abandons the resolution ceiling (70% autonomous)
               May be hard to return to after reframing
  Best if:     Team is exhausted; a clear win matters more than
               the original vision right now

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RECOMMENDATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

RECOMMENDED DIRECTION: A — "Fix the Floor"
  Because: The primary bottleneck (hallucinated policies) explains
           most of the resolution gap. This is solvable and the
           solution (RAG grounding) is well-understood.
  vs Direction B: Parallel build adds coordination risk during a
                  low-confidence phase. Fix the primary issue first.
  vs Direction C: Narrowing to routing permanently lowers the ceiling
                  and wastes the progress already made on resolution.
  Assumption: Policy documents can be structured for retrieval (not
              in unstructured PDFs requiring heavy processing).
  If that assumption is wrong: → Direction C becomes more viable
                                  while document processing is solved

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
THE ONE THING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  ▶ Add retrieval-augmented generation to the resolution agent:
    chunk and index the 3 core policy documents, replace the
    current "answer from memory" prompt with a retrieve-then-answer
    pattern, and measure resolution rate on the same 50-ticket
    test set used last week.

  Why this one:
    It directly addresses the primary bottleneck (hallucinated policy).
    All other improvements — feedback loops, new categories, prompting
    — are lower leverage until this is working.

  How to know it's done:
    Resolution rate on test set moves from 40% to ≥60%.
    Manual review of 20 resolved tickets shows no incorrect policy citations.

  When to revisit direction:
    After resolution rate reaches 60%+ → run compass again to
    determine whether to expand categories or focus on closing
    the remaining gap to 70%.

╔══════════════════════════════════════════════════════════════╗
║  MOMENTUM:  Declining → Building (after The One Thing)       ║
║  CEILING:   High                                             ║
║  DIRECTION: A — Fix the Floor                                ║
╚══════════════════════════════════════════════════════════════╝
```
