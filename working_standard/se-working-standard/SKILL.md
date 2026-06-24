---
name: se-working-standard
description: >
  Apply software engineering principles as working standards when executing any
  project task. This skill guides HOW an agent works — not whether it works.
  Use this skill at the start of any non-trivial project task to establish working
  standards, and refer to it throughout execution to maintain quality decisions.
  Trigger phrases: "build this properly", "engineer this well", "follow SE standards",
  "apply best practices", "do this the right way", "production-quality", "set up a
  project", "start a new feature", "implement this", or any task where the agent will
  write code, design systems, create plans, or make architectural decisions.
  These 8 principles are not a scoring rubric — they are the agent's operating
  standards for doing good work on any project.
---

# Agent Engineering Skill

Eight software engineering principles that guide how to approach and execute work.
Read the relevant principles before starting a task. Apply them as you work.

## Core Principle

> Good engineering is not about doing more work.
> It is about making the right decisions at each step
> so the result is reliable, secure, and maintainable —
> not just "done."

---

## The 8 Working Principles

### 1. Reliability — Build things that work under real conditions

**What this means when working:**
Design every component to handle failure. Assume external services will fail.
Assume inputs will be malformed. Assume the happy path is not the only path.

**Apply when:**
- Writing any function that calls an external API → add error handling + retry logic
- Processing user input → validate and handle malformed data explicitly
- Writing multi-step workflows → define what happens if step 3 fails
- Storing or transmitting data → ensure consistency even on failure

**Working standards:**
- Every error path is handled explicitly — no silent failures, no bare `except`
- Retries use exponential backoff with a maximum limit
- Functions return meaningful errors, not just `null` or `false`
- State that must survive a crash is persisted, not kept only in memory
- Timeouts exist on every external call

**Ask yourself while working:**
"What happens when this fails? Is that acceptable behavior?"
If the answer is "it crashes" or "nothing" → fix it before moving on.

---

### 2. Security & Trust — Handle data and access with minimum necessary privilege

**What this means when working:**
Every piece of code, config, or plan that touches sensitive data or access controls
must be written defensively. Security is not a feature added at the end — it is
a constraint respected from the first line.

**Apply when:**
- Handling credentials, tokens, or secrets → never hardcode, always environment/vault
- Accepting user input → validate and sanitize before use
- Designing access controls → grant minimum privilege needed, no more
- Writing logs → ensure PII and secrets are excluded
- Making API calls → confirm the scope is limited to what's needed

**Working standards:**
- Secrets live in environment variables or a secrets manager, never in code or prompts
- User-provided input is never trusted — validate type, length, format, range
- Logs contain what happened, not the sensitive data that was processed
- Every external call uses the minimum permission scope required
- Irreversible operations (delete, send, publish) have a confirmation gate

**Ask yourself while working:**
"If this code were leaked or this config were exposed, what would be compromised?"
If the answer is "credentials" or "user data" → restructure before proceeding.

---

### 3. Efficiency — Use minimum resources to accomplish the task correctly

**What this means when working:**
Do not make an API call you already made. Do not read a file you already read.
Do not run sequentially what can run in parallel. Efficiency is not premature
optimization — it is not being wasteful.

**Apply when:**
- Making repeated calls for the same data → cache the result
- Running independent subtasks → parallelize where possible
- Writing loops that process large datasets → use streaming or batching
- Choosing algorithms → pick one appropriate for the scale, not just "works"

**Working standards:**
- Data fetched once per session unless it changes — cache results in scope
- Independent operations run in parallel (Promise.all, asyncio.gather, etc.)
- No operation repeated that has already been done and whose result is still valid
- Algorithm choice considers the scale: O(n²) fine for n=100, not for n=10,000
- Context passed to sub-steps contains only what that step needs

**Ask yourself while working:**
"Have I already done this? Could I do multiple things at once here?"
If the answer is yes → refactor before adding more steps.

---

### 4. Scalability — Design for growth, not just for today

**What this means when working:**
Write code and design systems that still work when the load is 10× higher,
the dataset is 10× larger, or the number of users doubles. You don't need to
over-engineer for scale that doesn't exist yet — but you should not create
hard limits that will require a full rewrite when things grow.

**Apply when:**
- Storing data → use a structure that works at 10× the current volume
- Designing APIs → pagination from the start, not added later
- Writing database queries → ensure they use indexes, not full scans
- Handling concurrent operations → no shared mutable state between parallel workers
- Choosing data structures → lists for small N, sets/maps for lookup-heavy use

**Working standards:**
- Pagination on every list endpoint — never return unbounded result sets
- No global mutable state that would break under concurrent access
- Database queries are indexed for the access pattern used
- Configuration values (batch size, timeout, limits) are configurable, not hardcoded
- No assumption that a file, queue, or dataset will stay small

**Ask yourself while working:**
"What breaks at 10× the current load? Is that acceptable?"
If a full-table scan is acceptable at 1,000 rows, is it at 100,000?

---

### 5. Maintainability — Write for the person who reads this next

**What this means when working:**
Code, config, and documentation should be understandable by someone who
wasn't there when it was written. That person is often you, six months later.
Maintainability is not about adding comments to everything — it's about making
intent clear and making changes safe.

**Apply when:**
- Naming variables, functions, classes → names should reveal intent
- Writing complex logic → add a comment explaining *why*, not just *what*
- Making architectural decisions → write a brief note explaining the choice
- Structuring code → each function/module does one thing
- Adding configuration → document what each value controls and what's safe to change

**Working standards:**
- Function names say what the function does: `fetch_user_orders()` not `get_data()`
- Complex decisions have a comment explaining the reasoning: `# Using X instead of Y because...`
- No magic numbers — named constants with descriptive names
- Each function has a single responsibility — if it does two things, split it
- The README or inline docs explain how to run, modify, and extend the code

**Ask yourself while working:**
"If I came back to this in 6 months with no context, would I understand it?"
If the answer is no → add clarity before moving on.

---

### 6. Cost-effectiveness — Spend resources proportional to value delivered

**What this means when working:**
Not every task needs the most powerful tool, the most expensive API tier,
or the most complex architecture. Use the right resource for the job.
Know what things cost and make conscious tradeoffs.

**Apply when:**
- Choosing a model or API → match capability to task complexity
- Designing a workflow → batch non-urgent work, don't process one item at a time
- Caching → avoid re-running expensive operations for the same inputs
- Storing data → don't keep what you don't need
- Making API calls → use bulk/batch endpoints where available

**Working standards:**
- Model selection matches task: use a lighter model for classification/routing
- Expensive operations (LLM calls, external APIs) are cached where inputs repeat
- Batch endpoints used for non-real-time processing
- Data retained only as long as needed — no indefinite storage of transient outputs
- Tradeoffs between cost and quality are made explicitly, not by default

**Ask yourself while working:**
"Is this the right tool for this specific step, or am I using it out of habit?"
If the answer is habit → consider whether a cheaper option fits.

---

### 7. User Satisfaction — Solve the real problem, not just the stated one

**What this means when working:**
The literal request is not always the actual need. Good work solves what the
user actually needs, in a form they can actually use, with honesty about
what the result can and cannot do.

**Apply when:**
- Interpreting a request → check if the literal interpretation is what's actually needed
- Choosing output format → match the format to how the user will consume it
- Handling uncertainty → flag it clearly rather than papering over it
- Delivering results → explain what was done and what to watch out for
- Hitting a limitation → surface it and suggest alternatives

**Working standards:**
- Output format matches use case: don't return JSON when prose is needed, or vice versa
- Uncertainty is flagged explicitly: "This relies on X being true — verify before use"
- When the request has a better approach, say so: "You asked for X; Y would serve this better"
- Output includes what the user needs to act on it, not just the raw result
- Do not present incomplete results as complete without flagging the gap

**Ask yourself while working:**
"Will the person receiving this output be able to use it directly?
Is there anything they need to know that I haven't told them?"

---

### 8. Innovation — Use the best available approach, not the first one that comes to mind

**What this means when working:**
The first solution you think of is not always the best one. Modern tools,
patterns, and capabilities often provide simpler, more reliable solutions
than manual implementations. Stay current, but adopt thoughtfully.

**Apply when:**
- Implementing something manually → check if a library or built-in does this better
- Using a tool or pattern → check if a better alternative exists
- Solving a recurring problem → look for established solutions before inventing one
- Designing a workflow → consider if a newer approach eliminates steps

**Working standards:**
- Check for built-in or standard solutions before implementing from scratch
- Evaluate trade-offs before adopting a new tool: capability, complexity, cost
- Don't use a complex solution when a simple one does the job
- Don't use a legacy pattern when a modern equivalent is cleaner
- Document why a specific approach was chosen when alternatives exist

**Ask yourself while working:**
"Is there a better way to do this that I haven't considered?
Am I solving a problem that's already been solved?"

---

## How to Apply These Principles

### At the Start of a Task

Identify which principles are most relevant:

```
TASK TYPE → HIGHEST PRIORITY PRINCIPLES
──────────────────────────────────────────────────────
Writing code           → Reliability, Security, Maintainability
Designing a system     → Scalability, Reliability, Cost-effectiveness
Processing data        → Efficiency, Reliability, Security
Making a plan          → User Satisfaction, Reliability, Maintainability
Calling external APIs  → Reliability, Security, Efficiency, Cost
Building a pipeline    → All 8, in the order listed above
```

Identify the 2–3 most critical principles for this specific task.
Read only those sections above (not all 8 every time).

### During the Task

At each significant decision point, ask the relevant principle's question:
- Writing error handling → "What happens when this fails?"
- Writing a query → "Does this work at 10× the current scale?"
- Naming something → "Would someone new understand this instantly?"

### Before Finishing

Run the working checklist for the task type.
See `references/working-checklist.md`.

---

## Reference Files

- `references/principles-by-task.md` — Which principles matter most for each task type
- `references/working-checklist.md` — Checklist to run before completing any task
