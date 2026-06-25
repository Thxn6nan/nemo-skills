# nemo-skills-collection

A curated collection of Claude Code skills organized into two categories: working standards and service maintenance.

## Structure

```
nemo-skills-collection/
├── working_standard/          # How to do work well
│   ├── se-working-standards/  # 8 SE principles applied during any project task
│   ├── anti-hallucination/    # Detect and prevent fabricated outputs
│   ├── goal-evaluator/        # Evaluate whether a goal is worth pursuing
│   ├── project-compass/       # Find the best direction when a project drifts
│   ├── project-planner/       # Break down and plan project work
│   ├── project-qa/            # Enforce scope boundaries on project tasks
│   ├── tenth-man/             # Stress-test plans and decisions (10th Man Rule)
│   └── work-audit/            # Audit completed work for defects and gaps
│
└── maintain_service/          # How to keep a running service healthy
    ├── database-maintenance/  # Database health, migrations, and optimization
    ├── debugging-rootcause/   # Find the actual cause of bugs, not just symptoms
    ├── e2e-testing/           # End-to-end test strategy and execution
    ├── incident-response/     # Respond to and resolve production incidents
    ├── performance-scaling/   # Diagnose and fix performance bottlenecks
    ├── project-monitor/       # Monitor service health and detect issues
    ├── release-management/    # Plan, coordinate, and execute releases
    ├── retrospective/         # Run structured retrospectives after incidents or sprints
    ├── security-audit/        # Audit code and systems for vulnerabilities
    ├── system-design/         # Design systems with appropriate trade-offs
    ├── tech-debt-management/  # Identify and manage technical debt
    └── testing-strategy/      # Design and improve test coverage strategy
```

## Usage

Each skill lives in its own directory with a `SKILL.md` and optional `references/` folder containing supporting guides.

Install via the Claude Code skill registry or copy into your project's `.claude/skills/` directory. Invoke with `/skill-name` in Claude Code.

## Categories

### Working Standard

Skills that govern **how work gets done** on any project. Apply these at task start to establish quality standards.

| Skill | Purpose |
|-------|---------|
| `se-working-standards` | 8 core principles: reliability, security, efficiency, scalability, maintainability, cost-effectiveness, user satisfaction, innovation |
| `anti-hallucination` | Verify outputs and prevent fabricated facts, citations, or code |
| `goal-evaluator` | Assess whether a goal delivers real value before committing to it |
| `project-compass` | Reorient when a project loses direction or scope creep sets in |
| `project-planner` | Structured task breakdown and planning for non-trivial work |
| `project-qa` | Enforce scope boundaries — catch out-of-scope work before it starts |
| `tenth-man` | Red-team plans and decisions; surface failure modes consensus missed |
| `work-audit` | Audit delivered work for defects, gaps, and missed requirements |

### Maintain Service

Skills for **keeping a live service healthy** — from debugging to releases to security.

| Skill | Purpose |
|-------|---------|
| `debugging-rootcause` | Hypothesis-driven root cause analysis: reproduce → evidence → isolate → verify |
| `incident-response` | Structured response for production incidents: triage, mitigation, resolution |
| `security-audit` | Vulnerability checklist across OWASP top 10, secrets, access controls |
| `performance-scaling` | Diagnose bottlenecks, profile, and design scaling strategies |
| `database-maintenance` | Schema migrations, index health, query optimization, backups |
| `e2e-testing` | End-to-end test design, coverage gaps, and execution strategy |
| `testing-strategy` | Evaluate and improve overall test coverage and pyramid balance |
| `release-management` | Release planning, rollout sequencing, rollback procedures |
| `project-monitor` | Metrics, alerting, and health checks for running services |
| `retrospective` | Structured post-incident or sprint retrospective facilitation |
| `system-design` | Architecture decisions, trade-off analysis, and design documentation |
| `tech-debt-management` | Identify, prioritize, and pay down technical debt systematically |
