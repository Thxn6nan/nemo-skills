# Coverage Analysis Guide

---

## Part 1: What Coverage Metrics Actually Mean

### Line Coverage

```
WHAT IT MEASURES: % of source code lines executed during test runs
WHAT IT MISSES:   A line can be executed without testing all its behaviors

EXAMPLE:
  def get_discount(user_type, amount):
      if user_type == "premium":
          return amount * 0.90
      return amount

  test_get_discount_for_premium_user()  # covers line 2 and 3
  # Line 4 is never tested — 75% line coverage but a path is untested
```

### Branch Coverage

```
WHAT IT MEASURES: % of decision branches (if/else/case) taken during tests
WHAT IT CATCHES:  "The else branch was never tested"
BETTER THAN:      Line coverage — same line, different paths

EXAMPLE:
  def get_discount(user_type, amount):
      if user_type == "premium":      # Branch A (true) and Branch B (false)
          return amount * 0.90        # Branch A path
      return amount                   # Branch B path

  Branch coverage requires BOTH:
    test where user_type == "premium"   → Branch A
    test where user_type != "premium"   → Branch B
```

### Mutation Coverage (most meaningful)

```
WHAT IT MEASURES: % of code mutations caught by failing tests
HOW IT WORKS:     Tool modifies your code (changes + to -, == to !=, etc.)
                  Then runs your tests. If tests still pass → tests are weak.
WHAT IT CATCHES:  Tests that execute code but don't verify the behavior

EXAMPLE MUTATION:
  Original:     return amount * 0.90
  Mutated to:   return amount * 0.80   ← different discount
  If tests still pass → tests don't verify the actual discount value!

TOOLS: mutmut (Python), Stryker (JS/TS/Java), PITest (Java)
COST:  Slow — run on CI nightly, not on every commit
```

---

## Part 2: Coverage Analysis Workflow

### Step 1 — Generate the Report

```bash
# Python (pytest-cov)
pytest --cov=src --cov-report=html --cov-branch

# JavaScript (Jest)
jest --coverage --coverageThreshold='{}' 

# Go
go test ./... -coverprofile=coverage.out
go tool cover -html=coverage.out
```

### Step 2 — Filter by Risk Level

```
DO NOT look at overall coverage first.
DO look at high-risk code first.

In your coverage report, filter to:
  □ Business logic modules
  □ Financial calculations
  □ Authentication and authorization
  □ Data validation
  □ Error handling paths

In these areas, look for:
  □ Lines highlighted in RED (never executed)
  □ Branches highlighted in YELLOW (partially covered)
```

### Step 3 — Interpret Each Gap

For each uncovered line or branch, ask:

```
QUESTION 1: Is this code in a high-risk area?
  NO  → low priority; may be acceptable to leave uncovered
  YES → continue to Question 2

QUESTION 2: What behavior is not being tested?
  "This branch handles the case where the user has no payment method"
  If you can state the behavior → write a test for it

QUESTION 3: Why is this not tested?
  Hard to reach in tests → may indicate a design issue (tight coupling)
  Forgotten → write the test
  Intentionally skipped → document and exclude from coverage target
  Dead code → remove it

QUESTION 4: What is the consequence of this code being wrong?
  High consequence → must test
  Low consequence → document the decision to skip
```

### Step 4 — Prioritize What to Add

```
PRIORITY ORDER for adding tests:

1. HIGH-RISK uncovered branches (auth, financial, data integrity)
2. Error handling paths (these are most often untested)
3. Edge cases in core business logic
4. Happy paths in medium-risk code
5. Low-risk code (deprioritize or skip)

EFFORT ESTIMATE:
  Adding a test for an already-running application:
    Simple function:         15-30 minutes
    Function with setup:     30-60 minutes
    Requires refactoring:    2-4 hours (fix the design too)
    Dead code path:          0 minutes (delete the dead code)
```

---

## Part 3: Coverage Targets

### The Right Way to Set Targets

```
WRONG: "We need 80% overall coverage"
  Why wrong: hides WHERE the 80% is. You can hit 80% by covering
  boilerplate and leaving business logic untested.

RIGHT: "High-risk code is 90%+ branch coverage;
        medium-risk code is 70%+ branch coverage;
        low-risk code has no target"

SETTING TARGETS BY ZONE:
  Step 1: Classify your codebase:
    HIGH RISK:   business logic, security, financial
    MEDIUM RISK: API handlers, data access, utilities
    LOW RISK:    config reading, framework glue, generated code

  Step 2: Set coverage targets per zone
  Step 3: Measure per zone in CI (not just overall)
```

### Coverage Ratchet Pattern

```
A ratchet prevents coverage from decreasing:
  
  Step 1: Measure current coverage
  Step 2: Set CI to fail if coverage drops below current level
  Step 3: As coverage improves, the floor rises automatically
  
  This prevents new code from being added without tests,
  without requiring the team to immediately fix all existing gaps.

Implementation:
  Set --cov-fail-under to current level in CI.
  When coverage increases, update the threshold.
```

---

## Part 4: Coverage Red Flags

```
🚩 HIGH COVERAGE, STILL GETTING BUGS IN PRODUCTION
   Cause: tests execute the code but don't verify the behavior.
   Check: do your tests have meaningful assertions?
   Fix: add assertions; consider mutation testing to find weak tests.

🚩 100% COVERAGE
   Almost certainly means:
     - Low-risk code is being tested unnecessarily, OR
     - Tests are testing implementation details, OR
     - Assertions are weak (just testing "it ran without error")
   100% is not a reasonable target for any non-trivial system.

🚩 COVERAGE DROPS CONSISTENTLY WITH EACH SPRINT
   New code is not being tested.
   Fix: make tests a requirement for code review; use coverage ratchet.

🚩 COVERAGE IS HIGH IN ONE MODULE, 0% IN ANOTHER
   Likely: one developer writes tests, others don't.
   Fix: team standard for coverage per PR; visible per-module reporting.

🚩 ADDING TESTS THAT ONLY INCREASE COVERAGE (not confidence)
   Writing trivial tests to hit a number.
   Signal: new test doesn't test anything that could realistically fail.
   Fix: focus on risk, not metric.
```

---

## Part 5: Requirement Tracing (for Validation)

Map requirements to tests to prove validation coverage:

```
REQUIREMENT TRACING TABLE:
  Requirement                           Test(s) Covering It        Status
  ─────────────────────────────────    ────────────────────────    ──────
  User can log in with email+password  test_login_success           ✅
                                       test_login_wrong_password    ✅
                                       test_login_unknown_email     ✅
  
  Locked accounts cannot log in        test_login_locked_account    ✅
  
  Session expires after 30 minutes     test_session_expiry          ✅
  
  Users can reset password via email   test_password_reset_sends_email  ✅
                                       test_password_reset_token_expires ✅
                                       (test for invalid token)         ❌ MISSING

MISSING TESTS (from tracing):
  No test verifies that an expired reset token is rejected.
  → Add: test_password_reset_rejects_expired_token()

THIS IS VALIDATION:
  Each requirement has at least one test.
  If a requirement has no test → we cannot claim we validated it.
```
