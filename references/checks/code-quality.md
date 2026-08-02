---
type: check
layer: 1-universal
status: active
source: combined from anti-slop code-patterns.md + de-slop grading structure
---

# Check: code quality

Detect AI slop patterns in code. Flag each instance with the exact span and a specific fix. This is not a style guide — it is a signal check. The patterns below indicate code generated without understanding, not code that disagrees with your preferences.

## 1. Naming antipatterns

**Generic variable names** — appear in AI-generated code at high density:
- `data`, `result`, `temp`, `value`, `item`, `thing`, `obj`, `info`, `response`, `output`
- Fix: name what the variable represents. `userData` → `currentUser`. `result` → `validationError`.

**Generic function names** — describe action without object:
- `handleData()`, `processItems()`, `manageUsers()`, `doStuff()`, `run()`
- Fix: name should describe action + what it acts on. `handleData()` → `normalizeInboundWebhookPayload()`.

**Over-verbose names** that restate the obvious:
- `getUserDataFromDatabaseByUserIdAndReturnResult()` → `getUser(userId)`
- The signature already provides the context.

**Placeholder names in production code:**
- `foo`, `bar`, `baz`, `test1`, `test2`, `MyClass`, `MyFunction`
- These are never acceptable outside throwaway examples.

**Suffix slop** — specificity-free role tags:
- `UserHelper`, `DataManager`, `ItemHandler`, `ServiceUtil`
- Fix: name the specific responsibility. `UserHelper` → `SessionTokenValidator`.

## 2. Comment antipatterns

**Obvious comments** that restate what the code clearly does:
```python
# Bad
# Create a user
user = User()

# Increment the counter
counter += 1

# Return the result
return result
```
Rule: if the code is self-documenting, delete the comment.

**Generic TODOs** with no owner, scope, or reason:
```python
# Bad
# TODO: Implement this
# TODO: Add error handling
# TODO: Optimize this

# Better
# TODO(alice): Handle 429 rate-limit from upstream — retries added in #482 but not tested under load
```

**Comment blocks announcing sections** with decorative separators:
```python
########################################
# INITIALIZATION
########################################
```
Fix: use functions and clear structure instead.

**Over-explained simple logic** — comments longer than the code they describe:
```python
# Check if the user is authenticated by examining the session token
# and verifying it matches our stored tokens in the database
if session.token in valid_tokens:
    process_request()
```
Fix: write clear code. If a comment is needed, explain the business rule, not the syntax.

## 3. Structure antipatterns

**Unnecessary abstraction layers** — wrapping simple operations in classes or functions with no added value:
```python
# Slop: full class for a one-liner
class NumberAdder:
    def add(self, a, b):
        return a + b
adder = NumberAdder()
result = adder.add(5, 3)

# Better
result = 5 + 3
```

**Design patterns applied without cause** — Strategy, Factory, Observer on a 30-line script.

**Over-engineered error handling** — catching every possible exception with identical handling:
```python
# Slop
try:
    result = do_thing()
except ValueError:
    print("Error: ValueError")
    return None
except TypeError:
    print("Error: TypeError")
    return None
except Exception as e:
    print(f"Unexpected error: {e}")
    return None
```

**Defensive programming theatre** — validation for inputs that cannot take invalid values in context.

## 4. Implementation antipatterns

**Reinventing standard library functions:**
```python
# Slop
def find_max(numbers):
    max_val = numbers[0]
    for num in numbers:
        if num > max_val:
            max_val = num
    return max_val

# Better
max_val = max(numbers)
```

**Magic numbers without context:**
```python
# Slop
if user.score > 85:
    grant_access()

# Better
MINIMUM_TRUST_SCORE = 85
if user.score > MINIMUM_TRUST_SCORE:
    grant_access()
```

**Inconsistent patterns within the same codebase** — mixing idioms that serve the same purpose.

## 5. Documentation antipatterns

**Auto-generated docstrings** that only restate the function signature:
```python
def calculate_total(items):
    """
    Calculate the total.
    
    Args:
        items: The items to calculate
        
    Returns:
        The calculated total
    """
```
Fix: document the business rule, the non-obvious edge case, or nothing.

**Outdated comments** describing what the code used to do.

**README boilerplate** copied without adaptation: generic installation sections, placeholder "About this project" copy, TODOs describing what a README should contain.

## Grading

- Aligned: meaningful names throughout, comments explain why not what, structure matches actual complexity, no obvious reinventions.
- Drift: a few generic variable names, one or two obvious comments, minor over-engineering in one area.
- Misaligned: widespread generic naming, comment blocks restating code, significant unnecessary abstraction, or placeholder patterns in production code.
