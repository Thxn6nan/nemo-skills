# Unit Testing Guide — Methodology, Patterns, Anti-Patterns

---

## Part 1: Writing Good Unit Tests

### Naming Convention

Test names should read as specifications, not descriptions of code.

```
BAD NAMES (describe code):
  test_calculate()
  test_user_service_get_user()
  test_order_total_1()

GOOD NAMES (describe behavior):
  test_calculate_discount_returns_zero_when_no_items_in_cart()
  test_get_user_raises_not_found_when_user_id_does_not_exist()
  test_order_total_applies_10_percent_discount_for_premium_users()

NAMING TEMPLATE:
  test_[what is being tested]_[condition]_[expected behavior]

  What is being tested: the function or behavior
  Condition:            the specific input or state
  Expected behavior:    what should happen
```

### Test Size and Focus

```
ONE BEHAVIOR PER TEST:
  If a test has multiple assertions for multiple different behaviors,
  split it into multiple tests.

  BAD — tests multiple behaviors:
    def test_process_order():
        result = process_order(order)
        assert result.status == "confirmed"       # behavior 1
        assert result.total == 150.00             # behavior 2
        assert notification_sent == True          # behavior 3

  GOOD — each behavior gets its own test:
    def test_process_order_sets_status_to_confirmed():
        result = process_order(order)
        assert result.status == "confirmed"

    def test_process_order_calculates_correct_total():
        result = process_order(order)
        assert result.total == 150.00

    def test_process_order_sends_notification():
        process_order(order)
        assert notification_service.send.called

EXCEPTION: multiple assertions can be in one test when they all verify
  the same single behavior (e.g., checking multiple fields of one returned object).
```

### Edge Cases to Always Test

```
For any function, test these inputs unless irrelevant:
  □ Empty input (empty string, empty list, empty dict, 0)
  □ Single item (list with one element)
  □ Maximum / minimum values for numeric inputs
  □ None / null input (should it be handled or raise an error?)
  □ Negative numbers (if the domain allows them)
  □ Boundary values (off-by-one: length-1, length, length+1)
  □ Unicode / special characters for string inputs
  □ Concurrent access (if the function is used concurrently)
```

---

## Part 2: Isolation and Test Doubles

### Isolation Principle

```
A unit test tests ONE unit in isolation.
If your test requires a database, network, or file system → it's an integration test.

WHAT TO ISOLATE:
  □ External services (APIs, message queues, email)
  □ Databases (use in-memory fakes or mocks)
  □ File system (use temp files or mocks)
  □ Clock / time (inject time as a parameter or mock it)
  □ Random number generation (seed it or mock it)

WHAT NOT TO MOCK:
  □ Your own business logic (testing mocks tests nothing)
  □ Value objects and data structures (use real ones)
  □ Simple pure functions (no side effects, just use them)
```

### Choosing the Right Test Double

```python
# STUB — returns a fixed value, use when you just need the call to succeed
class StubEmailService:
    def send(self, to, subject, body):
        return True  # always succeeds; test doesn't care about this

# MOCK — verifies the interaction happened correctly
from unittest.mock import MagicMock
email_service = MagicMock()
# ... run code ...
email_service.send.assert_called_once_with(
    to="user@example.com",
    subject="Welcome",
    body=ANY
)

# FAKE — working implementation, simplified for testing
class FakeUserRepository:
    def __init__(self):
        self._users = {}
    
    def save(self, user):
        self._users[user.id] = user
    
    def find_by_id(self, user_id):
        return self._users.get(user_id)

# SPY — wraps real implementation to observe calls
class SpyLogger:
    def __init__(self, real_logger):
        self._real = real_logger
        self.calls = []
    
    def log(self, message):
        self.calls.append(message)
        self._real.log(message)  # still calls real logger
```

### Dependency Injection for Testability

```python
# HARD TO TEST — creates its own dependencies
class OrderService:
    def __init__(self):
        self.db = PostgresDatabase()        # hardcoded
        self.email = SmtpEmailService()     # hardcoded

# EASY TO TEST — dependencies injected
class OrderService:
    def __init__(self, db, email_service):  # injected
        self.db = db
        self.email = email_service

# In production:
service = OrderService(db=PostgresDatabase(), email_service=SmtpEmailService())

# In tests:
service = OrderService(db=FakeDatabase(), email_service=MockEmailService())
```

---

## Part 3: Testing Patterns

### Testing Error Cases

```python
# Test that errors are raised correctly
def test_get_user_raises_not_found_when_user_does_not_exist():
    repo = FakeUserRepository()  # empty repository
    service = UserService(repo)
    
    with pytest.raises(UserNotFoundError) as exc_info:
        service.get_user(user_id=999)
    
    assert exc_info.value.user_id == 999  # verify error has correct info

# Test that errors are handled correctly
def test_process_order_returns_failure_when_payment_declined():
    payment_service = StubPaymentService(response="declined")
    service = OrderService(payment=payment_service)
    
    result = service.process_order(order)
    
    assert result.status == "payment_failed"
    assert result.error == "Payment declined"
```

### Testing Time-Dependent Code

```python
# BAD — depends on actual clock
def test_is_subscription_active():
    sub = Subscription(expires_at=datetime(2025, 12, 31))
    assert sub.is_active()  # breaks depending on when test runs

# GOOD — inject time
def test_is_subscription_active_when_not_yet_expired():
    sub = Subscription(expires_at=datetime(2025, 12, 31))
    now = datetime(2025, 6, 1)  # controlled time
    
    assert sub.is_active(as_of=now) == True

def test_is_subscription_inactive_when_expired():
    sub = Subscription(expires_at=datetime(2025, 1, 1))
    now = datetime(2025, 6, 1)
    
    assert sub.is_active(as_of=now) == False
```

### Parameterized Tests

```python
# Instead of writing 5 nearly-identical tests, use parameterization
@pytest.mark.parametrize("price,discount_pct,expected", [
    (100, 0,  100),   # no discount
    (100, 10,  90),   # 10% discount
    (100, 50,  50),   # 50% discount
    (0,   10,   0),   # zero price
    (100, 100,  0),   # 100% discount
])
def test_apply_discount(price, discount_pct, expected):
    result = apply_discount(price, discount_pct)
    assert result == expected
```

---

## Part 4: Anti-Patterns

```
🚩 TESTING IMPLEMENTATION DETAILS
   Testing which private methods were called, not what the output is.
   Problem: tests break when code is refactored even if behavior is correct.
   Fix: test behavior and output, not internal structure.

🚩 TESTS WITH NO ASSERTIONS
   A test that runs code but asserts nothing will always pass.
   "It didn't crash" is not the same as "it worked correctly."
   Fix: every test must assert at least one meaningful outcome.

🚩 MAGIC TEST DATA
   test_process(user_id=42, amount=13.37)
   Why 42? Why 13.37? No one knows. The test is hard to understand.
   Fix: use named variables that explain the significance.
   user_id = ANY_VALID_USER_ID = 1
   amount = MINIMUM_CHARGEABLE_AMOUNT = 0.50

🚩 SETUP TOO LARGE
   A test that requires 50 lines of setup to run 3 lines of actual code.
   Signal: your unit under test has too many dependencies.
   Fix: refactor the unit, or extract shared setup into fixtures.

🚩 TESTING THE MOCK
   Arrange: mock.get_total.return_value = 100
   Act: result = mock.get_total()
   Assert: assert result == 100
   This tests that Python assignment works. It tests nothing about your code.
   Fix: only mock dependencies; test real code.

🚩 ORDER-DEPENDENT TESTS
   Test B passes only if Test A runs first.
   Problem: tests become fragile and hard to debug.
   Fix: each test sets up its own state independently.

🚩 SLEEPING IN TESTS
   time.sleep(2)  # wait for the async thing to finish
   Problem: slow, unreliable, hides real issues.
   Fix: use proper async test utilities, polling with timeout, or mock time.
```
