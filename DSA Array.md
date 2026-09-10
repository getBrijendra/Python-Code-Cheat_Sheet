If you're revising **DSA for interviews**, especially arrays/lists, it helps to learn the **patterns/techniques** rather than individual problems.

## 1. Array / List — Most Important Patterns

| Pattern                     | Core idea                                        | Typical problems                    |
| --------------------------- | ------------------------------------------------ | ----------------------------------- |
| **Two Pointers**            | Maintain two indices and move them intelligently | Pair Sum, 3Sum, remove duplicates   |
| **Sliding Window**          | Maintain a moving contiguous range               | Max sum subarray, longest substring |
| **Prefix Sum**              | Precompute cumulative sums                       | Range sum, subarray sum             |
| **Hashing / Frequency Map** | Store elements/counts for O(1) lookup            | Two Sum, duplicates, frequencies    |
| **Sorting + Scan**          | Sort first, then exploit ordering                | 3Sum, intervals, duplicates         |
| **Binary Search**           | Repeatedly halve search space                    | Search, lower bound, answer search  |
| **Fast & Slow Pointer**     | Two pointers moving at different speeds          | Cycle detection, linked lists       |
| **Kadane's Algorithm**      | Track best subarray ending at current index      | Maximum subarray                    |
| **Difference Array**        | Efficient range updates                          | Range increment/update problems     |
| **Monotonic Stack**         | Stack maintains increasing/decreasing order      | Next greater element, histogram     |
| **Heap / Priority Queue**   | Keep smallest/largest candidates                 | Top K, Kth largest                  |
| **Greedy**                  | Make locally optimal choices                     | Jump Game, interval scheduling      |
| **Divide & Conquer**        | Break problem into smaller pieces                | Merge Sort, inversion count         |

---

# 2. Two Pointers ⭐⭐⭐

One of the most important patterns.

### Opposite direction

```python
left = 0
right = len(nums) - 1

while left < right:
    if nums[left] + nums[right] == target:
        return [left, right]
    elif nums[left] + nums[right] < target:
        left += 1
    else:
        right -= 1
```

Usually requires **sorted data**.

Examples:

```text
Two Sum II
3Sum
Container With Most Water
Valid Palindrome
Remove Duplicates
```

### Same direction

```python
slow = 0

for fast in range(len(nums)):
    if nums[fast] != 0:
        nums[slow] = nums[fast]
        slow += 1
```

Useful for:

* Remove duplicates
* Move zeroes
* Partitioning
* In-place filtering

---

# 3. Sliding Window ⭐⭐⭐

Used when the problem talks about a **contiguous subarray/subsequence**.

### Fixed-size window

```python
window_sum = sum(nums[:k])
maximum = window_sum

for i in range(k, len(nums)):
    window_sum += nums[i]
    window_sum -= nums[i-k]
    maximum = max(maximum, window_sum)
```

Think:

```text
[1, 2, 3, 4, 5, 6]
 ↑-----↑
 window

   ↑-----↑
   window
```

Examples:

```text
Maximum Sum Subarray of Size K
Average of Subarrays
Maximum Number of Vowels in Substring
```

### Variable-size window

```python
left = 0

for right in range(len(nums)):
    # add nums[right]

    while condition_is_invalid:
        # remove nums[left]
        left += 1

    # current window = nums[left:right+1]
```

Examples:

```text
Longest Substring Without Repeating Characters
Minimum Size Subarray Sum
Longest Subarray with Sum <= K
```

---

# 4. Hash Map / Hash Set ⭐⭐⭐

When you see:

> "Have we seen this before?"

Think **hashing**.

```python
seen = set()

for x in nums:
    if x in seen:
        return True
    seen.add(x)

return False
```

### Frequency

```python
freq = {}

for x in nums:
    freq[x] = freq.get(x, 0) + 1
```

### Two Sum

```python
seen = {}

for i, x in enumerate(nums):
    complement = target - x

    if complement in seen:
        return [seen[complement], i]

    seen[x] = i
```

Typical complexity:

```text
Time:  O(n)
Space: O(n)
```

---

# 5. Prefix Sum ⭐⭐⭐

Useful when repeatedly asking:

> "What is the sum from index i to j?"

Create:

```python
prefix = [0]

for x in nums:
    prefix.append(prefix[-1] + x)
```

Then:

```python
sum_i_to_j = prefix[j + 1] - prefix[i]
```

Example:

```text
nums:
[2, 4, 1, 5, 3]

prefix:
[0, 2, 6, 7, 12, 15]
```

Sum `nums[1:4]`:

```python
prefix[4] - prefix[1]
= 12 - 2
= 10
```

---

# 6. Prefix Sum + Hash Map ⭐⭐⭐

Very important interview pattern.

Example:

> Find number of subarrays whose sum equals K.

```python
count = 0
current = 0
freq = {0: 1}

for x in nums:
    current += x

    count += freq.get(current - k, 0)

    freq[current] = freq.get(current, 0) + 1
```

This transforms an O(n²) problem into approximately:

```text
O(n)
```

---

# 7. Binary Search ⭐⭐⭐

Basic:

```python
left = 0
right = len(nums) - 1

while left <= right:
    mid = left + (right - left) // 2

    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        left = mid + 1
    else:
        right = mid - 1

return -1
```

But don't only learn "binary search on sorted array."

Learn:

### Binary Search on Answer

If the question asks:

> What is the minimum/maximum possible value satisfying a condition?

Think:

```text
Can I check whether answer X is possible?
        ↓
If yes → maybe smaller
If no  → need larger
```

Examples:

```text
Koko Eating Bananas
Capacity to Ship Packages
Minimum Days to Make Bouquets
Split Array Largest Sum
```

---

# 8. Kadane's Algorithm ⭐⭐

Maximum subarray sum.

```python
current = nums[0]
best = nums[0]

for x in nums[1:]:
    current = max(x, current + x)
    best = max(best, current)
```

Core idea:

```text
Should I:

start a new subarray at x

OR

extend the previous subarray?
```

---

# 9. Sorting + Two Pointers ⭐⭐⭐

Very common combination.

For example **3Sum**:

```python
nums.sort()

for i in range(len(nums)):
    left = i + 1
    right = len(nums) - 1

    while left < right:
        total = nums[i] + nums[left] + nums[right]

        if total == 0:
            ...
        elif total < 0:
            left += 1
        else:
            right -= 1
```

Pattern:

```text
Sort
 ↓
Fix one element
 ↓
Two pointers for remaining elements
```

---

# 10. Intervals ⭐⭐⭐

Another very popular array pattern.

Input:

```python
intervals = [
    [1, 3],
    [2, 6],
    [8, 10],
    [9, 12]
]
```

Usually:

```python
intervals.sort()

result = []

for start, end in intervals:
    if not result or start > result[-1][1]:
        result.append([start, end])
    else:
        result[-1][1] = max(result[-1][1], end)
```

Think:

```text
Sort by start
      ↓
Compare current with previous
      ↓
Merge / don't merge
```

Problems:

```text
Merge Intervals
Insert Interval
Meeting Rooms
Meeting Rooms II
Non-overlapping Intervals
```

---

# 11. Monotonic Stack ⭐⭐⭐

Used when the problem asks something like:

> What is the next greater/smaller element?

Example:

```python
stack = []
result = [-1] * len(nums)

for i, x in enumerate(nums):
    while stack and nums[stack[-1]] < x:
        j = stack.pop()
        result[j] = x

    stack.append(i)
```

Think:

```text
Need nearest
greater/smaller
        ↓
Monotonic Stack
```

Examples:

```text
Next Greater Element
Daily Temperatures
Largest Rectangle in Histogram
Stock Span
```

---

# 12. Heap / Priority Queue ⭐⭐⭐

When you hear:

```text
Top K
Kth largest
Kth smallest
smallest/largest repeatedly
```

Think **heap**.

Python:

```python
import heapq

heap = []

for x in nums:
    heapq.heappush(heap, x)
```

K largest:

```python
heapq.nlargest(k, nums)
```

Or maintain a size-K heap.

---

# 13. Linked List Patterns

For linked lists, memorize these patterns:

### Traversal

```python
current = head

while current:
    print(current.val)
    current = current.next
```

### Reverse Linked List

```python
prev = None
current = head

while current:
    nxt = current.next
    current.next = prev
    prev = current
    current = nxt

return prev
```

The three variables are critical:

```text
prev ← current → next

       ↓
reverse pointer

prev ← current    next
```

### Fast / Slow

```python
slow = head
fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
```

Used for:

```text
Find middle
Detect cycle
Find cycle entry
Palindrome linked list
```

---

# 14. Recursion / Backtracking

Think:

```text
Choose
 ↓
Explore
 ↓
Undo
```

Template:

```python
def backtrack(path):
    if condition:
        result.append(path.copy())
        return

    for choice in choices:
        path.append(choice)

        backtrack(path)

        path.pop()
```

Used for:

```text
Subsets
Permutations
Combinations
N-Queens
Sudoku
Word Search
```

---

# 15. Tree Patterns

For trees, know these extremely well:

### DFS

```python
def dfs(node):
    if not node:
        return

    dfs(node.left)
    dfs(node.right)
```

Three orders:

```text
Preorder:   Root → Left → Right
Inorder:    Left → Root → Right
Postorder:  Left → Right → Root
```

### BFS

```python
from collections import deque

queue = deque([root])

while queue:
    node = queue.popleft()

    if node.left:
        queue.append(node.left)

    if node.right:
        queue.append(node.right)
```

Used for:

```text
Level Order
Shortest path in unweighted tree
Tree width
Level-based calculations
```

---

# 16. Graph Patterns

The fundamental patterns are:

```text
DFS
BFS
Visited Set
Connected Components
Cycle Detection
Topological Sort
Shortest Path
Union Find
```

### DFS

```python
def dfs(node):
    if node in visited:
        return

    visited.add(node)

    for neighbor in graph[node]:
        dfs(neighbor)
```

### BFS

```python
queue = deque([start])
visited = {start}

while queue:
    node = queue.popleft()

    for neighbor in graph[node]:
        if neighbor not in visited:
            visited.add(neighbor)
            queue.append(neighbor)
```

---

# The DSA Pattern Map You Should Memorize

```text
ARRAY / STRING
│
├── Two Pointers
│   ├── Opposite direction
│   └── Same direction
│
├── Sliding Window
│   ├── Fixed
│   └── Variable
│
├── Hash Map / Set
│
├── Prefix Sum
│   └── Prefix Sum + Hash Map
│
├── Sorting + Scan
│
├── Binary Search
│   └── Binary Search on Answer
│
├── Kadane
│
├── Intervals
│
├── Monotonic Stack
│
└── Heap / Top-K
```

```text
LINKED LIST
│
├── Traversal
├── Reverse
├── Fast / Slow
├── Dummy Node
└── Merge
```

```text
TREE
│
├── DFS
│   ├── Preorder
│   ├── Inorder
│   └── Postorder
│
├── BFS / Level Order
├── Recursion
└── BST
```

```text
GRAPH
│
├── DFS
├── BFS
├── Visited
├── Connected Components
├── Cycle Detection
├── Topological Sort
├── Union Find
└── Shortest Path
```

```text
OTHER
│
├── Recursion
├── Backtracking
├── Dynamic Programming
├── Greedy
├── Heap
└── Bit Manipulation
```

### For interview preparation, I'd prioritize them in this order:

**1. Hash Map/Set → 2. Two Pointers → 3. Sliding Window → 4. Binary Search → 5. Prefix Sum → 6. Stack/Monotonic Stack → 7. Intervals → 8. Linked List/Fast-Slow → 9. Heap → 10. Trees/BFS/DFS → 11. Backtracking → 12. Graphs → 13. DP → 14. Greedy/Union-Find.**

The most important skill is learning to recognize the **trigger words in a problem statement** and map them to one of these patterns.
