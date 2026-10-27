# Global Variables in Python

In Python, a global variable is a variable defined outside a function. You can read it inside a function directly, but to reassign it inside a function, you generally need the `global` keyword.

## 1. Access a global variable inside a function

```
x = 10  # Global variable
def show():
    print(x)  # Read global variable
show()  # 10
```

You don't need `global` just to read a global variable.

## 2. Modify a global variable using `global`

```
x = 10
def update():
    global x
    x = 20
update()
print(x)  # 20
```

Key rule: Use `global x` when you want to reassign the module-level variable `x` from inside a function.

Without `global`, Python treats an assigned name as local within that function.

```
x = 10
def update():
    x = 20  # Creates a local variable
    print(x)  # 20
update()
print(x)  # 10
```

## 3. Common interview trap: `UnboundLocalError`

```
x = 10
def update():
    print(x)
    x = 20
update()  # UnboundLocalError
```

Why? Because Python determines that `x` is local to `update()` due to the assignment. The `print(x)` tries to read that local variable before it has been initialized.

Fix:

```
x = 10
def update():
    global x
    print(x)  # 10
    x = 20
update()
```

## 4. Modify a global list without `global`

You can mutate a global list without declaring it `global`, as long as you don't reassign the variable itself.

```
nums = [1, 2, 3]
def update():
    nums.append(4)  # Mutates the existing list
update()
print(nums)  # [1, 2, 3, 4]
```

But reassignment is different:

```
nums = [1, 2, 3]
def update():
    nums = [4, 5]  # Local variable;global list unchanged
update()
print(nums)  # [1, 2, 3]
```

## 5. `global` vs `nonlocal`

| Keyword    | Used for                                    |
| ---------- | ------------------------------------------- |
| `global`   | A variable defined at module scope          |
| `nonlocal` | A variable in an enclosing function's scope |

Example of `nonlocal`:

```
def outer():
    x = 10
    def inner():
        nonlocal x
        x = 20
    inner()
    print(x)  # 20
outer()
```

`nonlocal` does not refer to module-level variables.

## 6. Global variables in LeetCode solutions

Sometimes you may want to share a counter across recursive calls.

```
count = 0
class Solution:
    def dfs(self, node):
        global count
        count += 1
```

This works, but a global counter can accidentally retain its value between separate test cases or solution instances.

Better approach: Use an instance variable or a local variable when possible.

```
class Solution:
    def __init__(self):
        self.count = 0

    def dfs(self, node):
        self.count += 1
```

Or use a local variable passed through recursion, depending on the problem.


# Dict sorting
In Python, you can sort a dictionary by its values in descending order to get the keys with the highest values first.

```python
d = {'a': 10, 'b': 30, 'c': 20, 'd': 50}

sorted_dict = dict(sorted(d.items(), key=lambda x: x[1], reverse=True))

print(sorted_dict)
```

Output:

```python
{'d': 50, 'b': 30, 'c': 20, 'a': 10}
```

### If you only want the key with the highest value

```python
key = max(d, key=d.get)
print(key)  # d
```

### If you want all keys from highest to lowest

```python
keys = sorted(d, key=d.get, reverse=True)
print(keys)  # ['d', 'b', 'c', 'a']
```


# isinstance checking in python

```python
name = "Alice"
age = 30

print(isinstance(name, str))  # True
print(isinstance(age, int))   # True
print(isinstance(age, str))   # False

value = 3.14

# Using a tuple
print(isinstance(value, (int, float, str)))  # True

# Using a union operator (Python 3.10+)
print(isinstance(value, int | float))        # True

class Animal:
    pass

class Dog(Animal):
    pass

my_dog = Dog()

print(isinstance(my_dog, Dog))     # True
print(isinstance(my_dog, Animal))  # True (Inheritance is checked)

```


### isinstance() vs type()

Feature	 -->   isinstance(obj, Class)  -->  type(obj) == Class

Inheritance	 -->    Considers subclasses (returns True for parents)  -->	Checks the exact class only

Multiple Types	-->  Supports checking a tuple of types at once --> Requires individual or statements

Best Practice -->	Recommended for most runtime type checking  -->	Reserved for strict, exact match requirements


```python
from collections.abc import Iterable, Sequence

# Check if you can loop over it (strings, lists, dicts, etc.)
print(isinstance("hello", Iterable))     # True
print(isinstance([1, 2, 3], Iterable))   # True

# Check if it has an order and length (lists, tuples, NOT sets or dicts)
print(isinstance({1, 2, 3}, Sequence))   # False (Sets have no order/index)
print(isinstance((1, 2), Sequence))      # True
```

