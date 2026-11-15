# Pytest + `unittest.mock` Cheatsheet 🧪

## 1. What is Pytest?

**Pytest** is a Python testing framework used to automatically verify that your code works correctly.

```bash
pip install pytest
pytest
```

Pytest automatically finds:

```text
test_*.py
test_*()
```

Example:

```python
def add(a, b):
    return a + b


def test_add():
    assert add(2, 3) == 5
```

---

# 2. Basic Test Structure — AAA

Use **Arrange → Act → Assert**:

```python
def test_add():
    # Arrange
    a, b = 2, 3

    # Act
    result = add(a, b)

    # Assert
    assert result == 5
```

```text
Arrange → Prepare data
Act     → Run code
Assert  → Verify result
```

---

# 3. Useful Pytest Commands

```bash
pytest                    # Run all tests
pytest -v                 # Verbose output
pytest -x                 # Stop after first failure
pytest test_file.py       # Run one file
pytest test_file.py::test_name  # Run one test
```

---

# 4. Testing Exceptions

Use `pytest.raises()` when you expect an error.

```python
import pytest

def divide(a, b):
    return a / b


def test_divide_by_zero():
    with pytest.raises(ZeroDivisionError):
        divide(10, 0)
```

You can also check the error message:

```python
with pytest.raises(ValueError, match="User not found"):
    raise ValueError("User not found")
```

---

# 5. Fixtures ⭐

A **fixture provides reusable setup/data** for tests.

```python
import pytest


@pytest.fixture
def user():
    return {
        "name": "John",
        "age": 25,
    }


def test_user_name(user):
    assert user["name"] == "John"
```

Think:

```text
Fixture = reusable test setup
```

Common uses:

- Database/session
- Test user
- API client
- Authentication data
- Temporary files

---

# 6. Parametrize ⭐

Run the same test with multiple inputs.

```python
import pytest


@pytest.mark.parametrize(
    "a,b,expected",
    [
        (1, 2, 3),
        (5, 5, 10),
        (10, 20, 30),
    ],
)
def test_add(a, b, expected):
    assert a + b == expected
```

```text
One test function
      ↓
Multiple test cases
```

---

# 7. What is Mocking?

**Mocking replaces a real dependency with a fake one during testing.**

```text
Without Mock:

Service → Real Database/API/Email

With Mock:

Service → Fake Database/API/Email
```

Why?

- Faster tests
- No real API calls
- No database dependency
- No real emails/payments
- Predictable results

---

# 8. Create a Mock

```python
from unittest.mock import Mock

repository = Mock()
```

Configure a fake return value:

```python
repository.get_user.return_value = {
    "id": 1,
    "name": "John",
}
```

Now:

```python
repository.get_user(1)
```

returns:

```python
{"id": 1, "name": "John"}
```

---

# 9. Example: Service + Mock ⭐

### Production code

```python
class UserService:
    def __init__(self, repository):
        self.repository = repository

    def get_user_name(self, user_id):
        user = self.repository.get_user(user_id)
        return user["name"]
```

### Test

```python
from unittest.mock import Mock


def test_get_user_name():
    repository = Mock()

    repository.get_user.return_value = {
        "id": 1,
        "name": "John",
    }

    service = UserService(repository)

    result = service.get_user_name(1)

    assert result == "John"

    repository.get_user.assert_called_once_with(1)
```

```text
UserService → REAL ✅
Repository  → MOCK 🧪
```

---

# 10. `return_value` vs `side_effect`

## `return_value`

Always return a specific value:

```python
mock.get_user.return_value = "John"
```

```text
get_user() → "John"
```

## `side_effect`

### Raise an error

```python
mock.get_user.side_effect = ConnectionError()
```

### Return different values

```python
mock.get_user.side_effect = [
    "John",
    "Alice",
    "Bob",
]
```

```text
1st call → John
2nd call → Alice
3rd call → Bob
```

---

# 11. Verify Mock Calls ⭐

```python
mock.method.assert_called()
```

Called at least once.

```python
mock.method.assert_called_once()
```

Called exactly once.

```python
mock.method.assert_called_with(1, 2)
```

Last call used these arguments.

```python
mock.method.assert_called_once_with(1, 2)
```

Called exactly once with these arguments.

```python
mock.method.assert_not_called()
```

Never called.

---

# 12. `Mock` vs `MagicMock`

```python
from unittest.mock import Mock, MagicMock
```

### `Mock`

Normal fake object.

```python
mock = Mock()
```

### `MagicMock`

Supports Python special methods automatically:

```text
__len__
__iter__
__getitem__
__enter__
__exit__
```

**Simple rule:**

```text
Normal dependency → Mock
Special Python behavior → MagicMock
```

For most beginner/backend tests, `Mock` is enough.

---

# 13. `patch()` ⭐⭐⭐

`patch()` temporarily replaces a **real object/function** with a mock.

### Production

```python
# service.py

from payment import charge


def buy_product():
    charge(100)
    return "Success"
```

### Test

```python
from unittest.mock import patch


@patch("service.charge")
def test_buy_product(mock_charge):
    result = buy_product()

    assert result == "Success"
    mock_charge.assert_called_once_with(100)
```

---

## Golden Rule 🏆

> **Patch where the object is looked up/used.**

```text
payment.py
    ↓
defines charge()

service.py
    ↓
from payment import charge

service.charge() ← PATCH THIS
```

Use:

```python
patch("service.charge")
```

---

# 14. `patch()` Context Manager

Instead of a decorator:

```python
from unittest.mock import patch


def test_buy_product():
    with patch("service.charge") as mock_charge:

        result = buy_product()

        assert result == "Success"
        mock_charge.assert_called_once_with(100)
```

```text
Before `with` → Real object
Inside `with` → Mock
After `with`  → Real object restored
```

---

# 15. Mocking an API Call

### Production

```python
import requests


def get_temperature():
    response = requests.get("https://api.example.com")
    return response.json()["temperature"]
```

### Test

```python
from unittest.mock import patch


def test_get_temperature():

    with patch("weather.requests.get") as mock_get:

        mock_get.return_value.json.return_value = {
            "temperature": 25
        }

        result = get_temperature()

        assert result == 25
```

Mental model:

```text
requests.get()
      ↓
   response
      ↓
response.json()
      ↓
    data
```

Mock version:

```python
mock_get.return_value
# fake response

mock_get.return_value.json.return_value
# fake data
```

---

# 16. Dependency Injection vs `patch()`

## Dependency Injection

```python
class UserService:
    def __init__(self, repository):
        self.repository = repository
```

Test:

```python
repository = Mock()
service = UserService(repository)
```

You **pass the mock yourself**.

## `patch()`

```python
with patch("module.dependency"):
    ...
```

You **temporarily replace an existing dependency**.

### Simple rule

```text
Clean architecture/service dependencies
        ↓
Prefer Dependency Injection

Direct imports/external functions
        ↓
Use patch()
```

---

# 17. Common Mistakes ⚠️

### ❌ Patching the wrong location

Always remember:

```text
PATCH WHERE IT IS USED/LOOKED UP
```

### ❌ Testing the mock instead of your code

```python
mock.method.return_value = 10
assert mock.method() == 10
```

This mostly tests your mock configuration.

Instead, test real business logic using mocks as dependencies.

### ❌ Mocking everything

```text
Test real business logic
Mock external dependencies
```

---

# 18. FastAPI Testing Basics

```python
from fastapi import FastAPI
from fastapi.testclient import TestClient

app = FastAPI()


@app.get("/hello")
def hello():
    return {"message": "Hello"}


client = TestClient(app)


def test_hello():
    response = client.get("/hello")

    assert response.status_code == 200
    assert response.json() == {
        "message": "Hello"
    }
```

---

# 🧠 Final Cheat Sheet

| Concept | Meaning |
|---|---|
| `assert` | Check expected result |
| `pytest.raises()` | Test expected exceptions |
| `fixture` | Reusable test setup |
| `parametrize` | Run test with multiple inputs |
| `Mock()` | Create a fake dependency |
| `return_value` | Define fake return value |
| `side_effect` | Define errors/multiple/custom behavior |
| `assert_called_*` | Verify mock interactions |
| `MagicMock()` | Mock with special Python methods |
| `patch()` | Temporarily replace a real dependency |
| Dependency Injection | Pass dependency explicitly |
| `TestClient` | Test FastAPI endpoints |

## 🏆 Core Mental Model

```text
UNIT TEST

Arrange
  ↓
Create mocks + test data
  ↓
Act
  ↓
Run REAL code
  ↓
Assert
  ├── Check result
  └── Check mock interactions
```

**Golden rule:**

> **Keep your business logic real. Mock external dependencies.**
