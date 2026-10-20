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
