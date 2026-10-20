# Python String Methods — Interview Cheat Sheet

Python strings are immutable sequences of Unicode characters. String methods return a new string or another result; they don't modify the original string.

```
s = "Hello World"print(s.lower())  # hello worldprint(s)          # Hello World (unchanged)
```

## 1. Case conversion methods

| Method         | Example                      | Output          |
| -------------- | ---------------------------- | --------------- |
| `lower()`      | `"HELLO".lower()`            | `'hello'`       |
| `upper()`      | `"hello".upper()`            | `'HELLO'`       |
| `capitalize()` | `"hello WORLD".capitalize()` | `'Hello world'` |
| `title()`      | `"hello world".title()`      | `'Hello World'` |
| `swapcase()`   | `"HeLLo".swapcase()`         | `'hEllO'`       |
| `casefold()`   | `"Straße".casefold()`        | `'strasse'`     |

Interview tip: Use `casefold()` for caseless Unicode comparisons.

```
"HELLO".casefold() == "hello".casefold()  # True
```

## 2. Searching and finding

| Method          | Purpose                                    | Example                             |
| --------------- | ------------------------------------------ | ----------------------------------- |
| `find(x)`       | First index, or `-1`                       | `"banana".find("na")` → `2`         |
| `rfind(x)`      | Last index, or `-1`                        | `"banana".rfind("na")` → `4`        |
| `index(x)`      | First index; raises `ValueError` if absent | `"hello".index("e")` → `1`          |
| `rindex(x)`     | Last index; raises `ValueError` if absent  | `"banana".rindex("a")` → `5`        |
| `count(x)`      | Number of non-overlapping occurrences      | `"banana".count("an")` → `2`        |
| `startswith(x)` | Checks prefix                              | `"hello".startswith("he")` → `True` |
| `endswith(x)`   | Checks suffix                              | `"hello".endswith("lo")` → `True`   |

```
s = "programming"print(s.find("gram"))       # 3print(s.find("xyz"))        # -1print(s.count("m"))         # 2print(s.startswith("pro")) # True
```

Common trick: `find()` is safer when a substring might not exist; `index()` is useful when absence should raise an error.

## 3. Removing whitespace and characters

| Method            | Behavior             | Example                                      |
| ----------------- | -------------------- | -------------------------------------------- |
| `strip()`         | Removes both ends    | `"  hi  ".strip()` → `'hi'`                  |
| `lstrip()`        | Removes from left    | `"  hi  ".lstrip()` → `'hi  '`               |
| `rstrip()`        | Removes from right   | `"  hi  ".rstrip()` → \`'  hi'               |
| `removeprefix(x)` | Removes exact prefix | `"unhappy".removeprefix("un")` → `'happy'`   |
| `removesuffix(x)` | Removes exact suffix | `"file.txt".removesuffix(".txt")` → `'file'` |

Important distinction:

```
"www.example.com".strip("w.com")# 'example'  -- removes any matching characters at either end"www.example.com".removeprefix("www.")# 'example.com' -- removes the exact prefix
```

`strip(chars)` treats its argument as a set of characters, not a substring.

## 4. Splitting and joining

| Method            | Example                   | Output              |
| ----------------- | ------------------------- | ------------------- |
| `split(sep)`      | `"a,b,c".split(",")`      | `['a', 'b', 'c']`   |
| `rsplit(sep, n)`  | `"a-b-c".rsplit("-", 1)`  | `['a-b', 'c']`      |
| `splitlines()`    | `"a\nb".splitlines()`     | `['a', 'b']`        |
| `join(iterable)`  | `"-".join(["a","b"])`     | `'a-b'`             |
| `partition(sep)`  | `"a=b".partition("=")`    | `('a', '=', 'b')`   |
| `rpartition(sep)` | `"a=b=c".rpartition("=")` | `('a=b', '=', 'c')` |

```
s = "  python   is fun  "print(s.split())       # ['python', 'is', 'fun']print(s.split(" "))    # Includes empty strings for repeated spacesprint(" ".join(["I", "code"]))  # I code
```

Interview tip: `split()` without an argument groups consecutive whitespace and ignores leading/trailing whitespace. `split(" ")` uses the literal space delimiter.

## 5. Replacing and translating

| Method                 | Example                       | Output            |
| ---------------------- | ----------------------------- | ----------------- |
| `replace(old, new)`    | `"banana".replace("a", "o")`  | `'bonono'`        |
| `replace(old, new, n)` | `"aaaa".replace("a", "b", 2)` | `'bbaa'`          |
| `maketrans()`          | Creates a translation mapping | Mapping object    |
| `translate(table)`     | Applies character mapping     | Translated string |

```
# Replace multiple characters in one passtable = str.maketrans({"a": "1", "e": "2", "i": "3"})print("hello".translate(table))  # h2llo# Delete characterstable = str.maketrans("", "", "aeiou")print("beautiful".translate(table))  # btfl
```

## 6. Checking character types

These methods return Boolean values.

| Method           | Checks                           | Example                            |
| ---------------- | -------------------------------- | ---------------------------------- |
| `isalpha()`      | Letters only                     | `"Hello".isalpha()` → `True`       |
| `isdigit()`      | Unicode digits                   | `"123".isdigit()` → `True`         |
| `isdecimal()`    | Decimal digits                   | `"123".isdecimal()` → `True`       |
| `isnumeric()`    | Numeric characters               | `"½".isnumeric()` → `True`         |
| `isalnum()`      | Letters or numbers               | `"abc123".isalnum()` → `True`      |
| `isspace()`      | Whitespace only                  | `" \t\n".isspace()` → `True`       |
| `islower()`      | Has lowercase cased letters only | `"hello".islower()` → `True`       |
| `isupper()`      | Has uppercase cased letters only | `"HELLO".isupper()` → `True`       |
| `istitle()`      | Title-case pattern               | `"Hello World".istitle()` → `True` |
| `isidentifier()` | Valid Python identifier          | `"my_var".isidentifier()` → `True` |
| `isascii()`      | ASCII characters only            | `"hello!".isascii()` → `True`      |
| `isprintable()`  | Printable characters only        | `"hello".isprintable()` → `True`   |

### Important interview trap: `isdigit()` vs `isdecimal()` vs `isnumeric()`

```
print("123".isdecimal())  # Trueprint("²".isdecimal())   # Falseprint("²".isdigit())     # Trueprint("½".isdigit())     # Falseprint("½".isnumeric())   # True
```

For ordinary integer input validation, `isdecimal()` is often a better fit than `isnumeric()`. All three return `False` for an empty string.

## 7. Alignment and formatting

| Method          | Example                      | Output      |
| --------------- | ---------------------------- | ----------- |
| `center(width)` | `"hi".center(6, "-")`        | `'--hi--'`  |
| `ljust(width)`  | `"hi".ljust(5, "-")`         | `'hi---'`   |
| `rjust(width)`  | `"hi".rjust(5, "-")`         | `'---hi'`   |
| `zfill(width)`  | `"42".zfill(5)`              | `'00042'`   |
| `format()`      | `"Hi, DIL0".format("Sam")` | `'Hi, Sam'` |

\| `format_map()` | \`"".format_map(
