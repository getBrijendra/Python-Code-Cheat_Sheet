## Max and min int in python
        INT_MIN = -(1 << 31)
        INT_MAX = ((1 << 31) - 1)

## get sign of numbers
        negSign = (dividend > 0) != (divisor > 0)


# Python Bit Manipulation & Integer Overflow Cheat Sheet

A practical LeetCode cheat sheet for bit tricks, 32-bit integer boundaries, overflow handling, and common interview patterns.

One important Python fact: Python integers have arbitrary precision, so normal integer arithmetic does not overflow at 32 bits. However, LeetCode problems often ask you to simulate a fixed-width integer environment.

## 1. Integer limits and overflow snippets

For a signed 32-bit integer:

```
INT_MIN = -(1 << 31)       # -2147483648
INT_MAX = (1 << 31) - 1    #  2147483647
# Equivalent boundary constants
MIN_32 = -2**31
MAX_32 =  2**31 - 1
```

| Integer type  | Minimum       | Maximum        |
| ------------- | ------------- | -------------- |
| Signed 8-bit  | \\(-2^7\\)    | \\(2^7-1\\)    |
| Signed 16-bit | \\(-2^{15}\\) | \\(2^{15}-1\\) |
| Signed 32-bit | \\(-2^{31}\\) | \\(2^{31}-1\\) |
| Signed 64-bit | \\(-2^{63}\\) | \\(2^{63}-1\\) |

### Common overflow patterns

A. Clamp a result to 32-bit signed range

```
result = max(INT_MIN, min(result, INT_MAX))
```

B. Handle the special division overflow

```
if dividend == INT_MIN and divisor == -1:    return INT_MAX
```

C. Check before adding

```
# Check whether a + b would exceed INT_MAX
if b > 0 and a > INT_MAX - b:
        # Positive overflow
        pass

# Check whether a + b would fall below INT_MIN
if b < 0 and a < INT_MIN - b:
        # Negative overflow
        pass
```

D. Simulate signed 32-bit wraparound

```
MASK = (1 << 32) - 1
# Keep only the lowest 32 bits
x &= MASK

# Interpret as signed 32-bit
if x >= (1 << 31):
        x -= (1 << 32)
```

Wraparound is not the same as saturation. Use the behavior required by the problem.

## 2. Essential bit operators

| Operator | Meaning     | Example        |
| -------- | ----------- | -------------- |
| `&`      | Bitwise AND | `5 & 3 == 1`   |
| `\\|`    | Bitwise OR  | `5 \\| 3 == 7` |
| `^`      | XOR         | `5 ^ 3 == 6`   |
| `~`      | Bitwise NOT | `~5 == -6`     |
| `<<`     | Shift left  | `5 << 1 == 10` |
| `>>`     | Shift right | `5 >> 1 == 2`  |

For nonnegative integers, left shifting by \\(k\\) positions is equivalent to multiplying by \\(2^k\\), and right shifting is equivalent to floor-dividing by \\(2^k\\).

```
x << k      # Shift left k bits
x >> k      # Shift right k bits
1 << k      # Create a number with bit k set
```

For example, `1 << 4` is `16`, or binary `10000`.

## 3. Top 10 bit manipulation tricks

1\. Check whether a number is even or odd

```
if n & 1:
        # Odd
        pass
else:
        # Even
        pass
```

Why: the least significant bit is `1` for odd numbers.

2\. Check whether the k-th bit is set

```
if n & (1 << k):
        # Bit k is set
        pass
```

Bit positions start at `0`, from the right.

3\. Set the k-th bit

```
n |= (1 << k)
```

4\. Clear the k-th bit

```
n &= ~(1 << k)
```

5\. Toggle the k-th bit

```
n ^= (1 << k)
```

XOR with `1` flips a bit; XOR with `0` leaves it unchanged.

6\. Remove the lowest set bit

```
n &= n - 1
```

Example: `12` (`1100`) becomes `8` (`1000`).

7\. Isolate the lowest set bit

```
lowest = n & -n
```

Example: `12` (`1100`) gives `4` (`0100`).

8\. Check whether a number is a power of two

```
is_power_of_two = n > 0 and (n & (n - 1)) == 0
```

Powers of two have exactly one set bit.

9\. Count set bits (number of 1s)

```
count = 0while n:    n &= n - 1    count += 1
```

Time complexity: \\(O(k)\\), where \\(k\\) is the number of set bits, for nonnegative `n`.

10\. Check whether two numbers have opposite signs

```
opposite = (a < 0) != (b < 0)
```

This Boolean comparison is generally clearer in Python than checking sign bits.

## 4. XOR tricks for interview questions

XOR has three useful properties:

- `x ^ x == 0`
- `x ^ 0 == x`
- XOR is commutative and associative.

### Find the unique number

If every number appears twice except one:

```
nums = [4, 1, 2, 1, 2]ans = 0for n in nums:    ans ^= n# ans = 4
```

The duplicate values cancel each other.

Complexity: \\(O(n)\\) time, \\(O(1)\\) extra space.

### Swap two variables without a temporary variable

```
a ^= bb ^= aa ^= b
```

Python's normal swap is preferable in production:

```
a, b = b, a
```

## 5. Division without `/`, `*`, or `%`

This is the bit trick from LeetCode 29.

```
# Double a valuetemp <<= 1# Double the quotient contributioncount <<= 1# Subtract the largest possible chunkdividend -= temp# Add its contributionquotient += count
```

The key idea is:

\\[ \text{dividend} = q \times \text{divisor} + r \\]

Instead of subtracting the divisor one time per iteration, subtract the largest power-of-two multiple that fits.

For signed 32-bit division, remember:

```
INT_MIN = -(1 << 31)INT_MAX = (1 << 31) - 1if dividend == INT_MIN and divisor == -1:    return INT_MAX
```

Also determine the result's sign before converting operands to positive values.

## 6. Negative numbers and Python's bit behavior

This is a common interview trap.

```
print(~5)          # -6print(-5 >> 1)     # -3print(-5 & 1)      # 1
```

Why?

- Python represents negative integers conceptually with an infinite sign extension for bitwise operations.
- `~n` equals `-n - 1`.
- Right-shifting a negative integer rounds toward negative infinity, not toward zero.
- Python integers do not automatically truncate to 32 bits.

When a problem requires a fixed-width unsigned bit pattern, mask explicitly:

```
MASK = (1 << 32) - 1unsigned_32 = n & MASK
```

## 7. Bitmask patterns worth memorizing

| Goal                   | Snippet               |
| ---------------------- | --------------------- |
| Lowest `k` bits set    | `(1 << k) - 1`        |
| Keep lowest 8 bits     | `n & 0xFF`            |
| Keep lowest 16 bits    | `n & 0xFFFF`          |
| Keep lowest 32 bits    | `n & ((1 << 32) - 1)` |
| Clear lowest set bit   | `n & (n - 1)`         |
| Isolate lowest set bit | `n & -n`              |
| Set bit `k`            | `n \\| (1 << k)`      |
| Clear bit `k`          | `n & ~(1 << k)`       |
| Toggle bit `k`         | `n ^ (1 << k)`        |
| Check bit `k`          | `n & (1 << k)`        |

## 8. Quick practice checklist

Bit manipulation practice

0/9 completed

Reverse the bits of a 32-bit integer

Count the number of set bits

Find the missing number using XOR

Find the single number among duplicates

Check whether an integer is a power of two

Add two integers using bitwise operations

Divide two integers without multiplication, division, or modulo

Convert signed 32-bit to unsigned and back

Find two unique numbers when all others appear twice

My recommendation: Memorize the overflow boundaries, `n & (n - 1)`, `n & -n`, XOR cancellation, and the `1 << k` bitmask pattern first. Those patterns cover a large share of common bit-manipulation interview problems.
