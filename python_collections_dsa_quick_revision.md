# Python Collections & DSA — Quick Revision Sheet

## 1. List — `[]`

```python
# Create
a = []
a = [1, 2, 3]
a = [1, "hello", 3.5]

# Access
a[0]                 # first
a[-1]                # last
a[1:4]               # slice
a[::-1]              # reversed copy

# Add
a.append(4)          # add at end
a.insert(1, 10)      # insert at index
a.extend([5, 6])     # add multiple

# Update
a[0] = 100
a[1:3] = [20, 30]

# Delete
a.pop()              # remove last + return it
a.pop(2)             # remove index 2
a.remove(10)         # remove first occurrence of value
del a[0]             # delete index
del a[1:3]           # delete slice
a.clear()            # remove everything

# Search
x in a
x not in a
a.index(x)           # index of first x
a.count(x)           # occurrences

# Length
len(a)

# Min / Max / Sum
min(a)
max(a)
sum(a)
```

---

## 2. List Traversal

### Basic traversal

```python
a = [10, 20, 30]

for x in a:
    print(x)
```

### Index + value

```python
for i, x in enumerate(a):
    print(i, x)
```

### Index based

```python
for i in range(len(a)):
    print(a[i])
```

### Reverse traversal

```python
for x in reversed(a):
    print(x)
```

or:

```python
for i in range(len(a) - 1, -1, -1):
    print(a[i])
```

---

## 3. List Sorting

```python
a = [5, 2, 8, 1]

a.sort()                    # ascending
a.sort(reverse=True)       # descending

b = sorted(a)               # returns new list
b = sorted(a, reverse=True)
```

### Sort based on something else

```python
words = ["apple", "hi", "banana", "cat"]

words.sort(key=len)
```

Result:

```text
["hi", "cat", "apple", "banana"]
```

### Sort tuples by element

```python
a = [(1, 50), (2, 10), (3, 30)]

a.sort(key=lambda x: x[1])
```

Sort by second element.

### Descending based on second element

```python
a.sort(key=lambda x: x[1], reverse=True)
```

### Multiple sorting criteria

```python
a = [
    ("A", 20),
    ("B", 10),
    ("C", 20)
]

a.sort(key=lambda x: (x[1], x[0]))
```

Sort by:

1. second element
2. then first element

---

## 4. List Comprehension

```python
a = [1, 2, 3, 4]

b = [x * 2 for x in a]
```

With condition:

```python
b = [x for x in a if x % 2 == 0]
```

With `if/else`:

```python
b = [x if x > 0 else 0 for x in a]
```

Nested:

```python
b = [x for row in matrix for x in row]
```

---

## 5. List Inside List

```python
a = [
    [1, 2],
    [3, 4],
    [5, 6]
]
```

Access:

```python
a[0]       # [1, 2]
a[0][1]    # 2
```

Modify:

```python
a[1][0] = 100
```

Traverse:

```python
for row in a:
    for x in row:
        print(x)
```

With indexes:

```python
for i in range(len(a)):
    for j in range(len(a[i])):
        print(a[i][j])
```

---

## 6. 2D Array

### Correct allocation

```python
rows = 3
cols = 4

a = [[0 for _ in range(cols)] for _ in range(rows)]
```

or:

```python
a = [[0] * cols for _ in range(rows)]
```

Modify:

```python
a[1][2] = 10
```

### ⚠️ Avoid this

```python
a = [[0] * cols] * rows
```

Because all rows refer to the **same list**.

---

## 7. 3D Array

```python
x = 2
y = 3
z = 4

a = [[[0 for _ in range(z)]
      for _ in range(y)]
      for _ in range(x)]
```

Access:

```python
a[0][1][2]
```

Modify:

```python
a[0][1][2] = 99
```

Traverse:

```python
for i in range(x):
    for j in range(y):
        for k in range(z):
            print(a[i][j][k])
```

---

## 8. Tuple — `()`

Tuples are **immutable**.

```python
t = (1, 2, 3)

t[0]
t[-1]

len(t)
2 in t
```

Traversal:

```python
for x in t:
    print(x)
```

Unpacking:

```python
a, b, c = (10, 20, 30)
```

Swap:

```python
a, b = b, a
```

Tuple containing mutable object:

```python
t = ([1, 2], 10)

t[0].append(3)     # works
```

The tuple itself cannot change, but the list inside it can.

---

## 9. Dictionary — `{}`

```python
d = {}

d = {
    "name": "John",
    "age": 30
}
```

Access:

```python
d["name"]
```

Safer access:

```python
d.get("name")
d.get("salary", 0)
```

Add:

```python
d["city"] = "Delhi"
```

Update:

```python
d["age"] = 31
```

Delete:

```python
del d["age"]
d.pop("city")
d.clear()
```

Check:

```python
"name" in d
```

Keys:

```python
d.keys()
```

Values:

```python
d.values()
```

Key-value pairs:

```python
d.items()
```

Traverse:

```python
for key in d:
    print(key, d[key])
```

Better:

```python
for key, value in d.items():
    print(key, value)
```

---

## 10. Dictionary — Common DSA Patterns

### Frequency counter

```python
freq = {}

for x in nums:
    freq[x] = freq.get(x, 0) + 1
```

### Group values

```python
d = {}

for x in nums:
    if x not in d:
        d[x] = []

    d[x].append(x)
```

### Dictionary comprehension

```python
d = {x: x * x for x in range(5)}
```

---

## 11. Set — `{}`

```python
s = set()

s = {1, 2, 3}
```

Add:

```python
s.add(4)
```

Remove:

```python
s.remove(2)       # error if absent
s.discard(2)      # no error if absent
```

Check:

```python
3 in s
```

Length:

```python
len(s)
```

Operations:

```python
a | b             # union
a & b             # intersection
a - b             # difference
a ^ b             # symmetric difference
```

Very common DSA pattern:

```python
seen = set()

for x in nums:
    if x in seen:
        print("duplicate")

    seen.add(x)
```

---

## 12. Stack

Python list works perfectly as a stack.

```python
stack = []

stack.append(10)
stack.append(20)
stack.append(30)
```

Top:

```python
stack[-1]
```

Pop:

```python
x = stack.pop()
```

Check empty:

```python
if not stack:
    print("empty")
```

Typical:

```python
while stack:
    x = stack.pop()
```

---

## 13. Queue

Use `deque`, **not list**, for efficient front removal.

```python
from collections import deque

q = deque()

q.append(10)
q.append(20)
q.append(30)
```

Remove from front:

```python
x = q.popleft()
```

Front:

```python
q[0]
```

Check:

```python
if not q:
    print("empty")
```

---

## 14. Deque

```python
from collections import deque

d = deque()

d.append(10)          # right
d.appendleft(20)      # left

d.pop()               # remove right
d.popleft()           # remove left
```

Useful:

```python
d[0]
d[-1]
len(d)
```

Rotate:

```python
d.rotate(1)
d.rotate(-1)
```

---

## 14.1 heapq
Here’s a **small Python `heapq` cheat sheet** showing push, pop, min/max heap, and reverse order:

```python
import heapq

# Min Heap (smallest comes first)
heap = []

heapq.heappush(heap, 5)
heapq.heappush(heap, 2)
heapq.heappush(heap, 8)
heapq.heappush(heap, 1)

print(heap)              # [1, 2, 8, 5]

print(heapq.heappop(heap))  # 1
print(heapq.heappop(heap))  # 2

print(heap)              # [5, 8]


# Build heap from list
nums = [5, 2, 8, 1]
heapq.heapify(nums)

print(nums)              # [1, 2, 8, 5]


# Peek smallest (without removing)
print(nums[0])           # 1


# Max Heap — use negative values
max_heap = []

heapq.heappush(max_heap, -5)
heapq.heappush(max_heap, -2)
heapq.heappush(max_heap, -8)
heapq.heappush(max_heap, -1)

print(-heapq.heappop(max_heap))  # 8
print(-heapq.heappop(max_heap))  # 5
print(-heapq.heappop(max_heap))  # 2
print(-heapq.heappop(max_heap))  # 1
```

### Remember

| Operation           | Code                   |
| ------------------- | ---------------------- |
| Push                | `heapq.heappush(h, x)` |
| Pop smallest        | `heapq.heappop(h)`     |
| Peek smallest       | `h[0]`                 |
| Convert list → heap | `heapq.heapify(h)`     |
| Max heap push       | `heappush(h, -x)`      |
| Max heap pop        | `-heappop(h)`          |

**Reverse order / largest first:** Python `heapq` is naturally a **min-heap**, so use **negative numbers** for a max-heap.

---

## 15. `for` + `range`

```python
for i in range(5):
    print(i)
```

Produces:

```text
0 1 2 3 4
```

Start, stop:

```python
for i in range(2, 6):
    print(i)
```

Step:

```python
for i in range(0, 10, 2):
    print(i)
```

Negative:

```python
for i in range(10, 0, -1):
    print(i)
```

Negative numbers:

```python
for i in range(-5, 6):
    print(i)
```

---

## 16. `range` — Important Patterns

```python
range(n)
```

```python
range(start, stop)
```

```python
range(start, stop, step)
```

Examples:

```python
range(5)             # 0 1 2 3 4

range(2, 5)          # 2 3 4

range(10, 0, -1)     # 10 ... 1

range(-5, 5)         # -5 ... 4

range(5, -1, -1)     # 5 ... 0
```

Remember:

> **stop is excluded**

---

## 17. `reversed()`

```python
a = [1, 2, 3, 4]

for x in reversed(a):
    print(x)
```

Reverse list:

```python
a.reverse()
```

Create reversed copy:

```python
b = a[::-1]
```

---

## 18. Nested Loops

```python
for i in range(3):
    for j in range(4):
        print(i, j)
```

Three loops:

```python
for i in range(3):
    for j in range(3):
        for k in range(3):
            print(i, j, k)
```

---

## 19. Mix Collections + Loops

```python
data = {
    "A": [10, 20],
    "B": [30, 40]
}

for key, values in data.items():
    for value in values:
        print(key, value)
```

---

## 20. `max`, `min`, `sum`

```python
a = [10, 20, 5, 40]

max(a)
min(a)
sum(a)
```

Index of maximum:

```python
i = a.index(max(a))
```

### `max` with key

```python
students = [
    ("A", 80),
    ("B", 95),
    ("C", 70)
]

best = max(students, key=lambda x: x[1])
```

Result:

```python
("B", 95)
```

### Maximum based on second element

```python
max(a, key=lambda x: x[1])
```

---

## 21. `zip`

Very useful for interviews.

```python
a = [1, 2, 3]
b = ["a", "b", "c"]

for x, y in zip(a, b):
    print(x, y)
```

Create dictionary:

```python
d = dict(zip(a, b))
```

---

## 22. `enumerate`

```python
a = ["a", "b", "c"]

for i, x in enumerate(a):
    print(i, x)
```

Start from another number:

```python
for i, x in enumerate(a, start=1):
    print(i, x)
```

---

## 23. Multiple Assignment

```python
a, b = 10, 20
```

Swap:

```python
a, b = b, a
```

Tuple unpacking:

```python
x, y = (10, 20)
```

Ignore value:

```python
x, _, z = (10, 20, 30)
```

---

## 24. `if / elif / else`

```python
if x > 0:
    print("positive")
elif x < 0:
    print("negative")
else:
    print("zero")
```

One-line:

```python
result = "positive" if x > 0 else "negative"
```

---

## 25. `break`, `continue`, `pass`

```python
for x in nums:

    if x == 5:
        break
```

Skip:

```python
for x in nums:

    if x < 0:
        continue

    print(x)
```

Placeholder:

```python
if condition:
    pass
```

---

## 26. Infinite Loop

```python
while True:
    x = input()

    if x == "quit":
        break
```

Common DSA pattern:

```python
while left <= right:
    ...
```

---

## 27. Two Pointers

```python
left = 0
right = len(a) - 1

while left < right:

    if a[left] + a[right] == target:
        break

    elif a[left] + a[right] < target:
        left += 1

    else:
        right -= 1
```

---

## 28. Stack Pattern — Parentheses

```python
stack = []

for ch in s:

    if ch == "(":
        stack.append(ch)

    elif ch == ")":
        if not stack:
            return False

        stack.pop()

return not stack
```

---

## 29. Binary Tree — Basic Node

```python
class TreeNode:

    def __init__(self, val=0):
        self.val = val
        self.left = None
        self.right = None
```

Create:

```python
root = TreeNode(10)

root.left = TreeNode(5)
root.right = TreeNode(20)
```

Tree:

```text
       10
      /  \
     5    20
```

---

## 30. Binary Tree Traversal

### DFS — Preorder

```python
def preorder(root):

    if root is None:
        return

    print(root.val)

    preorder(root.left)
    preorder(root.right)
```

Order:

```text
Root → Left → Right
```

### Inorder

```python
def inorder(root):

    if root is None:
        return

    inorder(root.left)

    print(root.val)

    inorder(root.right)
```

Order:

```text
Left → Root → Right
```

### Postorder

```python
def postorder(root):

    if root is None:
        return

    postorder(root.left)
    postorder(root.right)

    print(root.val)
```

Order:

```text
Left → Right → Root
```

---

## 31. Binary Tree BFS

Use a queue.

```python
from collections import deque

def bfs(root):

    if not root:
        return

    q = deque([root])

    while q:

        node = q.popleft()

        print(node.val)

        if node.left:
            q.append(node.left)

        if node.right:
            q.append(node.right)
```

---

## 32. Tree — Level Order

```python
q = deque([root])

while q:

    for _ in range(len(q)):

        node = q.popleft()

        print(node.val)

        if node.left:
            q.append(node.left)

        if node.right:
            q.append(node.right)
```

The important pattern is:

```python
for _ in range(len(q)):
```

This processes **one level at a time**.

---

## 33. Graph — Adjacency List

```python
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0],
    3: [1]
}
```

Traverse neighbors:

```python
for neighbor in graph[node]:
    print(neighbor)
```

DFS:

```python
def dfs(node, graph, visited):

    if node in visited:
        return

    visited.add(node)

    for neighbor in graph[node]:
        dfs(neighbor, graph, visited)
```

BFS:

```python
q = deque([start])
visited = {start}

while q:

    node = q.popleft()

    for neighbor in graph[node]:

        if neighbor not in visited:
            visited.add(neighbor)
            q.append(neighbor)
```

---

## 34. Matrix Traversal

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

rows = len(matrix)
cols = len(matrix[0])

for i in range(rows):
    for j in range(cols):
        print(matrix[i][j])
```

---

## 35. Matrix — Four Directions

Extremely useful in grid problems.

```python
directions = [
    (-1, 0),   # up
    (1, 0),    # down
    (0, -1),   # left
    (0, 1)     # right
]
```

Then:

```python
for dr, dc in directions:

    nr = r + dr
    nc = c + dc

    if 0 <= nr < rows and 0 <= nc < cols:
        print(matrix[nr][nc])
```

---

## 36. Matrix — Eight Directions

```python
directions = [
    (-1, -1), (-1, 0), (-1, 1),
    (0, -1),           (0, 1),
    (1, -1),  (1, 0),  (1, 1)
]
```

---

## 37. Common Python DSA Shortcuts

```python
len(a)
```

```python
a.append(x)
```

```python
a.pop()
```

```python
a.sort()
```

```python
sorted(a)
```

```python
a.reverse()
```

```python
reversed(a)
```

```python
max(a)
min(a)
sum(a)
```

```python
x in a
```

```python
enumerate(a)
```

```python
zip(a, b)
```

```python
range(n)
```

```python
list(reversed(a))
```

```python
set(a)
```

```python
dict()
```

---

# 38. The Most Important Patterns to Memorize

If you're preparing for **coding interviews**, memorize these patterns first:

```python
# Traverse
for x in arr:
    ...

# Traverse with index
for i, x in enumerate(arr):
    ...

# Index loop
for i in range(len(arr)):
    ...

# Reverse
for i in range(len(arr)-1, -1, -1):
    ...

# Nested
for i in range(rows):
    for j in range(cols):
        ...

# Stack
stack.append(x)
x = stack.pop()

# Queue
q.append(x)
x = q.popleft()

# Set
if x in seen:
    ...
seen.add(x)

# Dictionary frequency
freq[x] = freq.get(x, 0) + 1

# Two pointers
left = 0
right = len(arr) - 1

while left < right:
    ...

# Binary search
left = 0
right = len(arr) - 1

while left <= right:
    mid = (left + right) // 2
    ...

# Tree DFS
if not root:
    return

dfs(root.left)
dfs(root.right)

# Tree BFS
q = deque([root])

while q:
    node = q.popleft()
    ...
```

---

# 39. One Mental Model

For most Python DSA questions, think in this order:

**Collection → Access → Modify → Traverse → Search → Sort → Choose data structure**

| Collection | Syntax | Main characteristic |
|---|---|---|
| List | `[]` | Ordered, mutable |
| Tuple | `()` | Ordered, immutable |
| Set | `set()` | Unique values |
| Dict | `{}` | Key → value |
| Stack | `list` | LIFO |
| Queue | `deque` | FIFO |
| Deque | `deque` | Both ends |
| Tree | `Node` | Hierarchical |
| Graph | `dict/list` | Relationships |
| Matrix | `list[list]` | 2D grid |

This gives you the syntax foundation needed for most **LeetCode-style Python DSA problems**.
