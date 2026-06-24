# Evaluation Report Format

---

## Full Report Template

```
╔══════════════════════════════════════════════════════════════╗
║              GOAL EVALUATION REPORT                          ║
╚══════════════════════════════════════════════════════════════╝

GOAL:      <stated goal in one sentence>
SCOPE:     <what is and is not included>
EVALUATOR: <who ran this evaluation>
DATE:      <ISO date>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 1 — CLARITY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Success definition:  <what does "done" look like — measurable>
Completion signal:   <how we know it's finished, not just better>
Core metric:         <the one number that moves if this works>
Core assumption:     <the bet this goal is based on>

Clarity status: [CLEAR | NEEDS REFINEMENT]
If NEEDS REFINEMENT → stop here and output REFINE with questions below.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 2 — VALUE ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Direct value:    <quantified where possible>
Indirect value:  <what this unlocks>
Strategic value: <direction alignment>
Inaction cost:   <what happens if we skip this>

Corrected value estimate (after optimism bias adjustment):
  Optimistic:   <X>
  Realistic:    <X>
  Pessimistic:  <X>
  Weighted:     <(O×0.25) + (R×0.50) + (P×0.25)>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 3 — COST ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Resource cost:     <time + money, with planning fallacy multiplier applied>
Opportunity cost:  <what won't happen; its estimated value>
Risk cost:         <P(failure) × cost of failing>
Reversal cost:     <cost if this turns out to be wrong>
Reversibility:     [Fully | Mostly | Partially | Irreversible]

Total realistic cost: <sum>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 4 — PROBABILITY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Base rate:             <% for this type of goal>
Team adjustment:       <+ or - % based on team factors>
Pre-mortem findings:   <top failure causes found>
Probability reduction: <% removed per present failure cause>

Adjusted probability:  <final %>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 5 — EXPECTED VALUE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

EV = (P(success) × value) − cost − (P(failure) × reversal cost)
   = (<P> × <V>) − <C> − (<1-P> × <RC>)
   = <result>

Best forgone alternative:  <what won't be done; its EV>
Adjusted EV (vs. alternative): <EV of this goal − EV of alternative>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 6 — BIAS CORRECTIONS APPLIED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Optimism bias:      [Detected | Not detected] — adjustment: <description>
Planning fallacy:   Multiplier applied: <×X>
Sunk cost:          [Present | Not present] — <description if present>
Motivated reasoning:[Checked | Risk flagged] — <note if flagged>
Scope sensitivity:  [Calibrated | Adjusted] — effective scope: <N people/instances>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 7 — FIT, TIMING & EXIT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Strategic fit:  [Strong | Moderate | Weak | Misaligned]
  Reason: <how this advances or conflicts with stated direction>

Timing:         [Now | Defer | Window closing | Arbitrary]
  Reason: <why now vs. waiting>
  If defer: trigger condition: <what must be true before revisiting>

Exit criteria:
  Success:       <measurable outcome that means done>
  Stop if:       <conditions that trigger abandonment>
  Review at:     <date or milestone>
  Minimum signal: <earliest evidence that validates or invalidates>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
VERDICT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

VERDICT: [PURSUE | REFINE | DEFER | DESCOPE | DROP]

Key finding:      <the single factor that most determined this verdict>
Primary risk:     <what most threatens success if pursuing>
Prerequisite:     <what must be true before starting — if any>
Exit condition:   <when to stop>

If REFINE: Questions that must be answered first:
  1. <question>
  2. <question>
  3. <question>

If DEFER: Resume when:
  <specific trigger or condition>
  Review date: <date>

If DESCOPE: The minimum viable version is:
  <description of smaller scope>
  Expected EV of smaller version: <estimate>

If DROP: Consider instead:
  <what would be a better use of this resource>

╔══════════════════════════════════════════════════════════════╗
║  VERDICT: [PURSUE|REFINE|DEFER|DESCOPE|DROP]                 ║
║  Adjusted EV: <positive/negative>   Confidence: [H|M|L]      ║
╚══════════════════════════════════════════════════════════════╝
```

---

## Worked Example — "Add real-time notifications to our app"

```
╔══════════════════════════════════════════════════════════════╗
║              GOAL EVALUATION REPORT                          ║
╚══════════════════════════════════════════════════════════════╝

GOAL:   Add real-time push notifications for new messages to the mobile app
SCOPE:  iOS and Android; message events only; excludes marketing/promo notifs
DATE:   2025-08-14

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 1 — CLARITY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Success definition:  Users receive a push notification within 5 seconds
                     of a new message being sent to them, on both iOS and Android
Completion signal:   End-to-end test passes for 95%+ of notification deliveries
Core metric:         Message response rate (currently 34%; target 50%+)
Core assumption:     Lack of notifications is the primary reason for slow responses
Clarity status: CLEAR

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 2 — VALUE ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Direct value:   If response rate goes from 34% → 50%:
                +16pp × ~8,000 messages/month = ~1,280 more responses/month
                Faster responses → higher user satisfaction → lower churn
                Estimated churn reduction: 2% → 1.5% (saves ~25 users/month)

Indirect value: Notification infrastructure enables future goals:
                order updates, reminders, re-engagement (estimated $15k value)

Strategic value: Messaging is core to product; strong fit

Inaction cost:  Competitor already has notifications; at risk of losing
                new users who expect this as table-stakes (~$3k/month risk)

Corrected value:
  Optimistic: $12k/month (full churn reduction + fast response uplift)
  Realistic:  $6k/month (partial; assumption about notifications as cause holds 60%)
  Pessimistic: $2k/month (notifications not main driver; minimal behavior change)
  Weighted: (12k×0.25) + (6k×0.5) + (2k×0.25) = $6,500/month

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 3 — COST ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Resource cost:   Initial estimate: 2 weeks (1 engineer)
                 Planning fallacy multiplier: ×1.5 (cross-platform, some novelty)
                 Corrected: 3 weeks = ~$9,000 in eng time
                 Ongoing: Firebase/APNs service ~$50/month

Opportunity cost: 3 weeks = the team's auth module refactor gets delayed.
                  Auth refactor value: ~$3k (security + tech debt reduction)

Risk cost:        P(failure) = 20% × cost of failed attempt ($9k) = $1,800

Reversal cost:    Mostly reversible — can disable notifications without
                  major code changes. Reversal cost: ~$2k if wrong.

Total realistic cost: $9k + $3k (opportunity) + $1.8k (risk) = ~$14k

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 4 — PROBABILITY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Base rate:        Cross-platform notification feature: 65% ship on schedule
Team adjustment:  +10% (team has done Firebase integration before)
Pre-mortem:       "Why might this fail?"
                  1. iOS APNs certificate setup takes longer than expected (+known risk)
                  2. Notification delivery rate lower than expected on Android
                  3. Users opt out of notifications at high rate (behavioral miss)
                  → Cause 3 (opt-out) is present now and not mitigable
                  → Reduce by 10%

Adjusted probability: 65% + 10% − 10% = 65%

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 5 — EXPECTED VALUE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Monthly value at 12-month horizon:
  EV = (0.65 × $6.5k × 12mo) − $14k − (0.35 × $2k)
     = $50,700 − $14,000 − $700
     = $36,000 net over 12 months

Best alternative (auth refactor): EV ~$3k in same period
Adjusted EV: $36k − $3k = +$33k vs. the alternative

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 6 — BIAS CORRECTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Optimism bias:      Detected and corrected — weighted value used ($6.5k vs $12k)
Planning fallacy:   ×1.5 multiplier applied — estimate moved from 2 to 3 weeks
Sunk cost:          Not applicable (nothing started yet)
Motivated reasoning: Core assumption checked — "is notification absence actually
                     the reason for slow responses?" No data yet. Flagged as risk.
Scope sensitivity:  8,000 messages/month confirmed as realistic volume

⚠️ FLAG: Core assumption (notifications = cause of slow response) is unvalidated.
          If false, realistic value drops to ~$2k/month (pessimistic case).

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 7 — FIT, TIMING & EXIT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Strategic fit:  Strong — messaging is core; notifications are table-stakes
Timing:         Now — competitor already has this; table-stakes gap growing

Exit criteria:
  Success:        Response rate reaches 48%+ within 60 days of launch
  Stop if:        Response rate unchanged after 30 days (suggest investigating
                  whether notifications were actually the blocker)
  Review at:      30 days post-launch
  Minimum signal: Opt-in rate >60% within first week → validates user interest

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
VERDICT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

VERDICT: PURSUE

Key finding:   EV is strongly positive even after bias corrections (+$33k vs.
               best alternative). Strategic fit is strong. Timing is justified.

Primary risk:  Core assumption (notifications = cause of low response) is
               unvalidated. If false, value is 3× lower. Monitor opt-in rate
               and response rate within 30 days as the early signal.

Prerequisite:  Run a 1-week survey asking inactive users why they don't respond
               quickly. If <30% cite "didn't know about it" → reconsider before
               committing full 3 weeks.

Exit condition: If response rate does not move by day 30, stop optimizing
                notifications and investigate alternative causes.

╔══════════════════════════════════════════════════════════════╗
║  VERDICT: PURSUE                                             ║
║  Adjusted EV: +$33,000 over 12 months   Confidence: MEDIUM  ║
╚══════════════════════════════════════════════════════════════╝
```
