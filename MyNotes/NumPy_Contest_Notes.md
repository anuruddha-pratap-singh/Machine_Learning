# 🎯 NumPy Contest Notes — Complete Revision

> **Source:** Every concept, example, and question below comes directly from the teacher's repository.
> **Goal:** If the teacher asks an MCQ about anything they taught in NumPy, these notes should give you the best possible chance of answering it correctly.

---

# 1. What is NumPy?

### What?
NumPy = **Numerical Python**. It gives you the `ndarray` — a fast, memory-efficient array for numbers.

### Why?
- Python lists are **slow** for math. NumPy arrays are **fast** because they use vectorization (no loops needed).
- Pandas and Sklearn are **built on top of NumPy**.

### Import
```python
import numpy as np
```

### 🧠 MCQ Facts
- `type(np.array([1,2,3]))` → `<class 'numpy.ndarray'>` — NOT `list`, NOT `array`
- NumPy arrays hold **one dtype only**. Lists can mix types.
- `np.__version__` gives the version string.

---

# 2. Creating Arrays

## 2.1 `np.array()` — from a list

```python
x = np.array([2, 3, 4, 5])
print(x.dtype)   # int64
print(type(x))   # <class 'numpy.ndarray'>
```

```python
x1 = np.array([[2,3],[4,5],[8,9]])
print(x1)
```
```text
[[2 3]
 [4 5]
 [8 9]]
```

> ⚠️ **MCQ TRAP — Type Upcasting**

```python
x = np.array([1, 2.5, 5.6])
print(x.dtype)   # float64
```
**Remember:** If you mix `int` and `float`, NumPy upcasts everything to `float64`.

```python
np.array([1, 2.5, "hello"])
# dtype becomes '<U32' (Unicode string) — everything becomes a string!
```

> 🔥 **IMPORTANT OUTPUT — Forced dtype truncates, does NOT round**

```python
x = np.array([1, 4, 4.3, 4.9, 4.8], dtype='int32')
print(x)
```
```text
[1 4 4 4 4]
```
**`4.9` → `4`, `4.8` → `4`.** `astype(int)` and `dtype=int` both **truncate** (chop off decimals), they do NOT round.

### 🎯 Remember
> `int()` in NumPy = floor towards zero, NOT `round()`.

---

## 2.2 `np.zeros()`, `np.ones()`, `np.full()`

```python
np.zeros(20)                      # 1D, 20 zeros, dtype float64
np.zeros(10, dtype='int32')       # 1D, 10 zeros, dtype int32
np.zeros((3, 4), dtype='int16')   # 2D, shape (3,4)
np.ones(10)                       # 1D, 10 ones, dtype float64
np.ones((3, 4))                   # 2D, shape (3,4)
np.full((4, 3), 15)               # 2D, every element = 15
```

```python
print(np.full((4,3), 15))
```
```text
[[15 15 15]
 [15 15 15]
 [15 15 15]
 [15 15 15]]
```

### 🧠 MCQ Facts
- `np.zeros(5)` → default dtype is `float64` → output has decimal points: `[0. 0. 0. 0. 0.]`
- `np.full((4,3), 15)` → dtype follows the fill value. `15` is int → dtype is `int64`.
- Shape must be a **tuple** for 2D: `np.zeros((3,4))` ✅ | `np.zeros(3,4)` ❌ Error

> ⚠️ **MCQ TRAP**: `np.zeros(5)` outputs `[0. 0. 0. 0. 0.]` (floats!), NOT `[0 0 0 0 0]`

---

## 2.3 `np.arange()` vs `np.linspace()`

### `np.arange(start, stop, step)` — stop is **EXCLUDED**

```python
print(np.arange(10))         # [0 1 2 3 4 5 6 7 8 9]
print(np.arange(10, 5, -2))  # [10  8  6]
```

### `np.linspace(start, stop, num)` — stop is **INCLUDED**

```python
print(np.linspace(0, 1, 5))  # [0.   0.25 0.5  0.75 1.  ]
```

### ⚖️ Comparison: `arange` vs `linspace`

| | `np.arange(start, stop, step)` | `np.linspace(start, stop, num)` |
|---|---|---|
| Stop value | **Excluded** | **Included** |
| You control | Step size | Number of points |
| Default dtype | `int` (if all args are int) | `float64` always |
| Main MCQ trap | Stop excluded | Stop included |

> 🔥 **IMPORTANT**: `np.arange(1, 10)` gives `int64`, but `np.linspace(1, 9, 9)` gives `float64` — even though values look the same!

### 🧠 MCQ Facts
- `np.arange(1, 5)` → `[1 2 3 4]` — **5 is NOT included**
- `np.linspace(0, 1, 5)` → `[0. 0.25 0.5 0.75 1.]` — **1 IS included**
- Gap formula for linspace: `gap = (stop - start) / (num - 1)`

### 🎯 Remember
> **arange** = like Python `range()` → stop excluded.
> **linspace** = "line space" → includes both endpoints.

---

## 2.4 `np.eye()` — Identity Matrix

```python
print(np.eye(3, 4))       # 3 rows, 4 cols, diagonal of 1s
print(np.eye(3, 4, k=2))  # diagonal shifted right by 2
```
```text
[[0. 0. 1. 0.]
 [0. 0. 0. 1.]
 [0. 0. 0. 0.]]
```

### 🧠 MCQ Facts
- Default dtype is `float64` → output shows `1.` not `1`
- `k=0` (default) = main diagonal, `k=1` = one above, `k=-1` = one below
- Use `np.eye(n, dtype=int)` to get integers

---

## 2.5 Chaining: `arange` + `reshape`

```python
print(np.arange(1, 13).reshape(4, 3))
```
```text
[[ 1  2  3]
 [ 4  5  6]
 [ 7  8  9]
 [10 11 12]]
```

> ⚠️ **MCQ TRAP — reshape size mismatch**

```python
np.arange(1, 13, 2).reshape(4, 3)
# ValueError: cannot reshape array of size 6 into shape (4,3)
```
`arange(1,13,2)` gives `[1 3 5 7 9 11]` → 6 elements, but 4×3 = 12 slots.

---

## 📝 Practice Set 1 — Array Creation (from repo)

1. Create a 1D array containing numbers from **1 to 10**.
2. Create a **3×3 matrix of all zeros**, and another of **all ones**.
3. Create an array of all **even numbers between 2 and 20** (inclusive) using `np.arange`.
4. Create **5 equally spaced values between 0 and 1** using `np.linspace`.
5. Create a **4×4 identity matrix** and print its `shape`, `size`, and `ndim`.
6. Create a 1D array of numbers 1–5 with dtype `float`.

**Answers:**
- Q1: `np.arange(1, 11)` — remember stop excluded!
- Q3: `np.arange(2, 21, 2)` — must go to 21 to include 20
- Q4: `np.linspace(0, 1, 5)` → `[0. 0.25 0.5 0.75 1.]`

---

# 3. Array Attributes

```python
arr = np.arange(5)
x = np.arange(20).reshape(4, 5)
```

| Attribute | `arr` (1D) | `x` (2D) | What it tells you |
|---|---|---|---|
| `.shape` | `(5,)` | `(4, 5)` | Dimensions as a tuple |
| `.ndim` | `1` | `2` | Number of dimensions |
| `.size` | `5` | `20` | Total number of elements |
| `.dtype` | `int64` | `int64` | Data type |
| `len()` | `5` | `4` | Length of **first** dimension only |

> ⚠️ **MCQ TRAP — `len()` vs `.size`**

```python
x = np.arange(20).reshape(4, 5)
print(len(x))    # 4  (number of rows, NOT total elements!)
print(x.size)    # 20 (total elements)
```

> ⚠️ **MCQ TRAP — Shape of 1D array**

```python
arr = np.arange(5)
print(arr.shape)  # (5,)  — NOT (5,1) and NOT (1,5)
```
**`(5,)` means 1D with 5 elements. The trailing comma is NOT a typo — it means "tuple with one element".**

### 🧠 MCQ Facts
- `.shape` returns a **tuple**, not a list
- `.dtype` is a **property**, not a method — no parentheses: `arr.dtype` ✅, `arr.dtype()` ❌
- `.size` = product of all shape dimensions: `(4,5)` → `size = 20`
- `len()` on 2D = number of **rows** only

---

# 4. Data Types & Type Conversion

## `dtype` and `astype()`

```python
x = np.array([1, 4, 4.3, 4.9, 4.8])
print(x)                # [1.  4.  4.3 4.9 4.8]  (auto-upcasted to float)
y = x.astype('int32')
print(y)                # [1 4 4 4 4]  (truncated!)
print(x)                # [1.  4.  4.3 4.9 4.8]  (ORIGINAL UNCHANGED!)
```

### 🧠 MCQ Facts
- `astype()` returns a **new array** — it does NOT modify the original
- Truncation rule: `4.9 → 4`, `4.3 → 4` — always towards zero
- Mixed int+float → float64 automatically
- Mixed anything+string → string (Unicode)

### 🎯 Remember
> `astype()` is **non-mutating**. You must save the result: `y = x.astype(int)`

---

# 5. Indexing & Slicing

## 5.1 1D Indexing

```python
arr = np.arange(10)   # [0 1 2 3 4 5 6 7 8 9]
print(arr[-3:-12:-2]) # [7 5 3 1]
```

**Remember:** Negative indexing works the same as Python lists. `-1` = last element.

## 5.2 1D Slicing — `[start:stop:step]`

- Stop is **excluded** (same as Python lists)
- `[::-1]` → reverses the array
- Default start = 0, default stop = end, default step = 1

## 5.3 2D Indexing — `arr[row, col]`

```python
arr = np.arange(20).reshape(5, 4)
print(arr)
```
```text
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]
 [12 13 14 15]
 [16 17 18 19]]
```

```python
print(arr[2, 2])   # 10 (single element)
```

## 5.4 2D Slicing — sub-matrix

```python
print(arr[1:3, 2::-1])
```
```text
[[ 6  5  4]
 [10  9  8]]
```
**Rows 1 to 2, columns 2 down to 0 (reversed).**

## 5.5 Boolean Indexing — `arr[condition]`

```python
arr = np.array([3, 4, 5, 10, 25, 56, 7, 8])
print(arr[arr > 10])   # [25 56]
```

> ⚠️ **MCQ TRAP — Multiple conditions need parentheses and `&`/`|`**

```python
marks = np.array([56, 91, 44, 77, 62, 85, 39, 70])
# Marks between 50 and 80:
result = marks[(marks >= 50) & (marks <= 80)]   # ✅
# result = marks[marks >= 50 & marks <= 80]     # ❌ Error!
```
**Each condition MUST be in parentheses when using `&` or `|`.**

---

## 📝 Practice Set 2 — Indexing & Slicing (from repo)

1. Array of 1–20, extract index 5 to 15.
2. Every alternate element.
3. Reverse using slicing only.
4. From a 4×4 matrix: 2nd row, 3rd column, middle 2×2 block.
5. Marks `[45, 82, 67, 33, 91, 58, 76]` — extract marks > 60.

**Q12 from Assignment 1** (important): From a 5×5 matrix, extract four corners as 2×2 using step slicing:
```python
m = np.array([[ 1,  2,  3,  4,  5],
              [ 6,  7,  8,  9, 10],
              [11, 12, 13, 14, 15],
              [16, 17, 18, 19, 20],
              [21, 22, 23, 24, 25]])
print(m[::4, ::4])
```
```text
[[ 1  5]
 [21 25]]
```

---

# 6. Basic Operations (Element-wise)

## 6.1 Array + Scalar (Broadcasting)

```python
arr1 = np.arange(10)
arr1 = arr1 + 5        # adds 5 to every element
print(arr1 * 5)        # multiplies every element by 5
print(arr1 ** 2)       # squares every element
```

## 6.2 Array + Array

```python
x = np.arange(10)
y = np.arange(11, 21)
print(x * y)           # element-wise multiplication
```
```text
[  0  12  26  42  60  80 102 126 152 180]
```

## 6.3 Comparison Operators → Boolean Arrays

```python
x = np.arange(5)       # [0 1 2 3 4]
y = np.arange(1, 6)    # [1 2 3 4 5]
print(x == y)          # [False False False False False]
print(x > y)           # [False False False False False]
print(y > x)           # [ True  True  True  True  True]
```

### ML Perspective
Element-wise operations = **vectorization**. This is why NumPy is used in ML — no loops, pure speed.

---

# 7. Broadcasting

## 7.1 The Rule

> Line up shapes **from the right**. Two dimensions are compatible when they are **equal** or when **one of them is 1**. A missing dimension counts as 1.

```text
(3, 3)  +  (3,)     → ok      (3,) becomes (1,3) → stretches to (3,3)
(3, 1)  +  (3,)     → ok      both stretch, result is (3, 3)
(4, 3)  +  (3,)     → ok      row applies to all 4 rows
(4, 3)  +  (4,)     → ERROR   3 vs 4 from the right — not compatible!
(3,)    +  (4,)     → ERROR   3 and 4 — not compatible
```

## 7.2 2D Matrix + 1D Row

```python
m = np.ones((3, 3))
row = np.array([10, 20, 30])
print(m + row)
```
```text
[[11. 21. 31.]
 [11. 21. 31.]
 [11. 21. 31.]]
```
**The row is applied to every row of the matrix.**

## 7.3 Column Vector + Row Vector → Grid!

```python
col = np.array([[1], [2], [3]])   # shape (3, 1)
row = np.array([10, 20, 30])      # shape (3,)
result = col + row
print(result.shape)   # (3, 3)
print(result)
```
```text
[[11 21 31]
 [12 22 32]
 [13 23 33]]
```

> ⚠️ **MCQ TRAP — Broadcasting Error**

```python
x = np.arange(6).reshape(2, 3)
y = np.arange(6).reshape(3, 2)
print(x + y)
# ValueError: operands could not be broadcast together with shapes (2,3) (3,2)
```
**(2,3) and (3,2) — compare from right: 3 vs 2 → FAIL**

## 7.4 Practical: Column-wise Standardization

```python
X = np.array([[170, 65], [155, 50], [180, 80], [165, 58]])
col_mean = X.mean(axis=0)    # [167.5, 63.25] — one per column
col_std = X.std(axis=0)
X_scaled = (X - col_mean) / col_std   # (4,2) - (2,) → broadcasting
```

### 🧠 MCQ Facts
- `axis=0` collapses **rows**, leaves one answer per **column**
- `axis=1` collapses **columns**, leaves one answer per **row**
- Broadcasting error message always shows both shapes — read them!

### 🎯 Remember
> `axis=0` = "down the rows" = column-wise result. `axis=1` = "across the columns" = row-wise result.

---

# 8. The `np.random` Module

## 8.1 `np.random.seed()` — Reproducibility

```python
np.random.seed(2)
print(np.random.rand(5))    # same 5 numbers every time with seed 2
```

### Why?
In ML, you need **reproducible** experiments. Same seed = same "random" numbers every time.

## 8.2 `rand` vs `randn` vs `randint`

| Function | Range | Distribution |
|---|---|---|
| `np.random.rand(n)` | `[0, 1)` | Uniform |
| `np.random.randn(n)` | `(-∞, +∞)` | Normal (mean=0, std=1) |
| `np.random.randint(low, high, size)` | `[low, high)` | Uniform integers |

```python
np.random.randint(10, 20, size=(5, 3))  # 5×3 matrix, values 10–19
```

> ⚠️ **MCQ TRAP**: `randint(10, 20)` → `high` value 20 is **EXCLUDED**. Range is 10–19.

## 8.3 `choice` and `shuffle`

```python
lst = [3, 4, 56, 7, 9]
np.random.choice(lst, 3, replace=False)   # pick 3 without repeats
np.random.choice(10, 5, replace=False)    # pick 5 from 0–9

arr = np.arange(10)
np.random.shuffle(arr)   # shuffles IN PLACE — returns None!
print(arr)
```

> ⚠️ **MCQ TRAP**: `np.random.shuffle()` returns **None**. It modifies the array in place.

```python
result = np.random.shuffle(arr)
print(result)   # None  (NOT the shuffled array!)
```

### 🧠 MCQ Facts
- `rand()` → values in `[0, 1)` — 0 is possible, 1 is NOT
- `randn()` → can be negative
- `randint(low, high)` → high excluded
- `shuffle()` → in-place, returns None
- `choice(replace=False)` → no duplicates

---

# 9. Basic Statistics

## 9.1 The Functions

```python
arr = np.arange(6).reshape(2, 3)
print(arr)
```
```text
[[0 1 2]
 [3 4 5]]
```

| Function | No axis | `axis=0` (col-wise) | `axis=1` (row-wise) |
|---|---|---|---|
| `arr.min()` | `0` | `[0 1 2]` | `[0 3]` |
| `arr.max()` | `5` | `[3 4 5]` | `[2 5]` |
| `arr.sum()` | `15` | `[3 5 7]` | `[3 12]` |
| `arr.mean()` | `2.5` | `[1.5 2.5 3.5]` | `[1. 4.]` |
| `np.median(arr)` | `2.5` | `[1.5 2.5 3.5]` | `[1. 4.]` |

```python
print(np.median(arr, axis=0))   # [1.5 2.5 3.5]
```

## 9.2 `argmax` vs `max`

```python
marks = np.array([72, 45, 88, 95, 30])
print(marks.max())       # 95 (the topper's MARKS)
print(marks.argmax())    # 3  (the topper's INDEX)
```

### 🎯 Remember
> **`max` = the value, `argmax` = the position.** Your teacher says: "the topper's marks vs the topper's roll number."

## 9.3 Axis Explained Visually

```text
[[1, 2, 3],     shape = (2, 3)
 [4, 5, 6]]     ndim = 2, rows = 2, columns = 3

axis=0: ↓ down the rows (result has 3 values — one per column)
  sum → [5, 7, 9]

axis=1: → across the columns (result has 2 values — one per row)
  sum → [6, 15]
```

> ⚠️ **MCQ TRAP — axis confusion**
> - `axis=0` does NOT mean "row-wise". It means "collapse axis 0 (rows)" → result is per COLUMN.
> - `axis=1` means "collapse axis 1 (columns)" → result is per ROW.

### 🧠 MCQ Facts
- `median` is only available as `np.median()`, NOT as `arr.median()`
- `std` and `var` → NumPy uses `ddof=0` by default (population std). Pandas uses `ddof=1`.
- When you don't specify axis → operates on ALL elements, returns a scalar

---

## 📝 Practice Set 3 — Random & Statistics (from repo)

1. Set `seed = 42`, simulate 10 dice rolls (1–6) using `randint`.
2. Generate 20 random marks (35–100). Find mean, max, min, std.
3. Create 3×4 random int matrix (10–50). Find column-wise mean, row-wise sum.
4. Find the index of the topper.
5. Randomly select 3 students from a list.

---

# 10. Reshaping & Dimensions

## 10.1 `reshape(rows, columns)`

**Golden Rule:** rows × columns must equal total elements.

```python
arr = np.arange(1, 13)
print(arr.reshape(3, 4))
```
```text
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]
```

> ⚠️ **MCQ TRAP — reshape does NOT modify in place**

```python
arr = np.arange(12)
arr.reshape(3, 4)     # does nothing to arr!
print(arr.shape)      # still (12,)
m = arr.reshape(3, 4) # must assign to a variable
```

> ⚠️ **MCQ TRAP — size mismatch**

```python
np.arange(12).reshape(3, 5)
# ValueError: cannot reshape array of size 12 into shape (3,5)
```

## 10.2 The `-1` Trick

`-1` means "NumPy, you calculate this dimension."

```python
arr = np.arange(1, 13)
print(arr.reshape(3, -1).shape)   # (3, 4) — NumPy computes 4
print(arr.reshape(-1, 2).shape)   # (6, 2) — NumPy computes 6
print(arr.reshape(-1, 1).shape)   # (12, 1) — single column
```

> ⚠️ **MCQ TRAP**: Only ONE dimension can be -1. `reshape(-1, -1)` → Error.

## 10.3 `reshape(-1, 1)` — The Most Important Reshape

```python
marks = np.array([65, 70, 88, 92])
print(marks.shape)              # (4,)
col = marks.reshape(-1, 1)
print(col.shape)                # (4, 1)
```
```text
[65 70 88 92]      →     [[65]
                           [70]
                           [88]
                           [92]]
```

### ML Perspective
Many ML libraries (Sklearn) require 2D input. `"Expected 2D array, got 1D array instead"` → fix with `reshape(-1, 1)`.

### Three shapes to remember forever:

| Shape | Meaning | Brackets |
|---|---|---|
| `(4,)` | 1D — no row/column concept | `[65 70 88 92]` |
| `(4, 1)` | 2D column — 4 rows, 1 col | `[[65] [70] [88] [92]]` |
| `(1, 4)` | 2D row — 1 row, 4 cols | `[[65 70 88 92]]` |

---

## 10.4 `flatten()` vs `ravel()`

Both convert any array to 1D. **The difference is copy vs view.**

```python
m = np.array([[1, 2, 3], [4, 5, 6]])

f = m.flatten()   # COPY — safe
r = m.ravel()     # VIEW — shares memory

f[0] = 999        # does NOT change m
r[1] = 888        # DOES change m!

print(m)
```
```text
[[  1 888   3]
 [  4   5   6]]
```

### ⚖️ Comparison: `flatten()` vs `ravel()`

| | `flatten()` | `ravel()` |
|---|---|---|
| Returns | **Copy** | **View** (usually) |
| Modify result → original? | **No** (safe) | **Yes** (dangerous) |
| Speed | Slower (copies memory) | Faster (no copy) |
| When to use | Default choice | Large arrays, read-only |

### 🎯 Remember
> **flatten = safe copy. ravel = risky view.** When in doubt, use `flatten()`.

---

## 10.5 `np.newaxis`

```python
marks = np.array([65, 70, 88, 92])

as_column = marks[:, np.newaxis]     # (4, 1) — same as reshape(-1, 1)
as_row = marks[np.newaxis, :]        # (1, 4) — same as reshape(1, -1)
```

### 🧠 MCQ Facts
- `[:, np.newaxis]` → adds axis **after** → column
- `[np.newaxis, :]` → adds axis **in front** → row
- `np.newaxis` is just `None` internally

---

# 11. Stacking & Combining Arrays

## 11.1 `np.vstack()` and `np.hstack()`

```python
A = np.array([[1, 2, 3], [4, 5, 6]])   # (2, 3)
B = np.array([[7, 8, 9]])               # (1, 3)

print(np.vstack((A, B)).shape)  # (3, 3) — rows added
```

```python
C = np.array([[10], [20]])   # (2, 1)
print(np.hstack((A, C)).shape)  # (2, 4) — columns added
```

### ⚖️ Comparison: `vstack` vs `hstack`

| | `vstack` | `hstack` |
|---|---|---|
| Direction | Vertical (rows grow) | Horizontal (columns grow) |
| Must match | Same number of **columns** | Same number of **rows** |
| axis equivalent | `axis=0` | `axis=1` |

> ⚠️ **MCQ TRAP — Tuple brackets!**

```python
np.vstack((A, B))    # ✅ — one argument (a tuple)
np.vstack(A, B)      # ❌ — TypeError: two arguments
```
**Same pattern as `np.zeros((3, 4))` — double brackets.**

## 11.2 The 1D `hstack` Surprise

> 🔥 **IMPORTANT OUTPUT**

```python
heights = np.array([170, 155, 180, 165])
weights = np.array([65, 50, 80, 58])

wrong = np.hstack((heights, weights))
print(wrong.shape)   # (8,)  — just glued end to end!

# Fix: reshape to columns first
X = np.hstack((heights.reshape(-1, 1), weights.reshape(-1, 1)))
print(X.shape)       # (4, 2)  — proper table!
```

### 🎯 Remember
> 1D arrays have no columns. `hstack` on 1D = concatenation. Use `reshape(-1, 1)` first to make a proper table.

## 11.3 `np.concatenate()`

The general-purpose version. You choose direction with `axis`.

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(np.concatenate((A, B), axis=0))  # same as vstack
print(np.concatenate((A, B), axis=1))  # same as hstack
```

### ⚖️ Comparison: `vstack/hstack` vs `concatenate`

| | `vstack/hstack` | `concatenate` |
|---|---|---|
| Direction | Function name decides | `axis` parameter decides |
| Readability | Clearer for 2D | Better for code where axis is variable |
| Use when | You know the direction | Direction comes from a variable |

---

# 12. Conditional Logic — `np.where`

## 12.1 Three-argument form: `np.where(condition, value_if_true, value_if_false)`

```python
marks = np.array([35, 68, 42, 91, 27, 55])
result = np.where(marks >= 40, "Pass", "Fail")
print(result)
```
```text
['Fail' 'Pass' 'Pass' 'Pass' 'Fail' 'Pass']
```

## 12.2 Data Cleaning with `np.where`

```python
arr = np.array([3, -7, 2, -1, 9, -4])
print(np.where(arr < 0, 0, arr))       # [3 0 2 0 9 0]

scores = np.array([88, 105, 96, 120, 74])
print(np.where(scores > 100, 100, scores))  # [ 88 100  96 100  74]
```

## 12.3 Nested `np.where`

```python
marks = np.array([92, 61, 45, 78, 33, 55])
grades = np.where(marks >= 75, 'A',
         np.where(marks >= 50, 'B', 'C'))
```

> ⚠️ **MCQ TRAP — Type mixing in np.where**
> `np.where(cond, "Pass", 0)` → everything becomes **string** because arrays hold one dtype.

## 12.4 Condition-only form: `np.where(condition)`

```python
marks = np.array([35, 68, 42, 91, 27, 55])
idx = np.where(marks > 50)[0]
print(idx)   # [1 3 5] — the POSITIONS, not values
```

> ⚠️ **MCQ TRAP**: Returns a **tuple** → use `[0]` to get the array of indices.

## 12.5 2D `np.where`

```python
m = np.array([[10, 80], [55, 20]])
rows, cols = np.where(m > 50)
print("rows:", rows, "cols:", cols)   # rows: [0 1] cols: [1 0]
print("values:", m[rows, cols])       # [80 55]
```

---

# 13. Copy vs View

## Slicing creates a **view** (shared memory)

```python
arr = np.arange(1, 11)
view = arr[2:6]       # VIEW — shares memory
copy = arr[2:6].copy() # COPY — independent

view[0] = 999         # CHANGES arr!
copy[0] = 999         # does NOT change arr
```

### 🧠 MCQ Facts
- Slicing → **view** → changes propagate to original
- `.copy()` → **copy** → independent, changes don't propagate
- `flatten()` → copy | `ravel()` → view
- `reshape()` → usually a view (changes can propagate!)

### 🎯 Remember
> If you don't want to risk changing the original, use `.copy()`.

---

# 14. Linear Algebra

## 14.1 Element-wise `*` vs Dot Product `@`/`np.dot()`

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

print(a * b)          # [ 4 10 18]  — element-wise
print(np.dot(a, b))   # 32  — 4+10+18 = single number
print(a @ b)          # 32  — same as np.dot
```

### ⚖️ Comparison: `*` vs `@`/`np.dot()`

| | `a * b` | `a @ b` / `np.dot(a,b)` |
|---|---|---|
| Operation | Element-wise multiply | Multiply AND sum |
| 1D result | Array of same size | Single number |
| 2D result | Element-wise matrix | Matrix multiplication |
| ML use | Feature scaling | Weighted scores, predictions |

## 14.2 Matrix Multiplication & Transpose

```python
x = np.array([[1, 2, 3], [4, 5, 6]])   # (2, 3)
print(x.T)        # Transpose: (3, 2)
print(x @ x.T)    # (2,3) @ (3,2) = (2,2) ✅
```

> ⚠️ **MCQ TRAP — Matrix multiplication shape rule**

For `A @ B` to work: **inner dimensions must match**.
- `(2,3) @ (3,2)` → ✅ → result is `(2,2)`
- `(2,3) @ (2,3)` → ❌ → 3 ≠ 2
- `A @ B ≠ B @ A` in general (matrix multiplication is NOT commutative)

### 🧠 MCQ Facts
- `A @ B.T` works when A and B have the **same** shape
- `A.T @ B` also works when A and B have the same shape
- `A @ B` with same shape only works if it's square
- `.T` is a property, not a method: `x.T` ✅, `x.T()` ❌

---

# 15. CRUD Operations on Arrays

## Delete & Insert (from Lab Practice)

```python
arr = np.array([10, 20, 30, 40, 50])

# Delete
np.delete(arr, 0)              # removes index 0 → [20 30 40 50]
np.delete(arr, -1)             # removes last → [10 20 30 40]

# Insert
np.insert(arr, 1, 50)          # inserts 50 at index 1

# Append
np.append(arr, 99)             # adds 99 at the end
```

### 🧠 MCQ Facts
- All three return a **new** array — original is unchanged
- For 2D: use `axis=0` to delete/insert rows, `axis=1` for columns
- Without axis on 2D → array is flattened first!

---

# 16. `np.diag()` — Two Roles

From Assignment 2, Q21:

```python
# Role 1: Extract diagonal from a matrix
m = np.arange(1, 17).reshape(4, 4)
print(np.diag(m))    # [1 6 11 16]

# Role 2: Build a diagonal matrix from a 1D array
print(np.diag([1, 2, 3]))
```
```text
[[1 0 0]
 [0 2 0]
 [0 0 3]]
```

---

# 17. `np.round()`

```python
print(np.round(3.14159, 2))   # 3.14
X_scaled = np.round(X_scaled, 2)
```

---

# 📝 Repository Questions with Answers

## Assignment 1 Key Questions

**Q3.** `np.arange(1, 10)` vs `np.linspace(1, 9, 9)` — same values, different dtype.
> **Answer:** `arange` → `int64`, `linspace` → `float64`.

**Q7.** Predict: `np.array([10.9, 25.5, 39.99]).astype(int)`
> **Answer:** `[10 25 39]` — truncation, not rounding.

**Q13.** Extract four corners of 5×5 matrix as 2×2 using step slicing:
> **Answer:** `m[::4, ::4]`

**Q15.** Extract marks between 50 and 80:
> **Answer:** `marks[(marks >= 50) & (marks <= 80)]` — parentheses required!

## Assignment 2 Key Questions

**Q3.** Predict shape of `np.arange(30).reshape(5, -1).T`:
> **Answer:** `(6, 5)` — reshape gives (5,6), transpose flips to (6,5).

**Q16.** `flatten()` vs `ravel()` behavior:
> **Answer:** `flatten` returns copy (original safe), `ravel` returns view (original changes).

**Q18.** Is `A @ B == B @ A`?
> **Answer:** No. Matrix multiplication is NOT commutative.

## Assignment 3 Key Questions

**Q2 Task 6.** Create 4×4 with 100 on diagonal, 35 elsewhere:
> **Answer:** `np.full((4,4), 35, dtype=int) + np.eye(4, dtype=int) * 65`

**Q10 Task 6.** Standardization:
> **Answer:** `(marks - marks.mean(axis=0)) / marks.std(axis=0)`

**Q12.** flatten vs ravel — copy vs view test.

**Q13 Task 7.** Attaching grades with nested `np.where` makes all values strings:
> Expected output: `[['72' '85' '60' 'B'] ...]` — numbers become strings!

**Q15.** Weighted score with `@`:
> `M @ w` gives one weighted score per student.
> `M @ W` with a weight matrix gives multiple scores.

---

# 🚨 LAST-MINUTE MCQ REVISION

## Shape Rules
- `(5,)` is 1D. NOT `(5,1)`.
- `(5,1)` is 2D column. `(1,5)` is 2D row.
- `reshape` requires rows × cols = total elements.
- `-1` in reshape = auto-calculate. Only ONE `-1` allowed.

## Axis Rules
- `axis=0` → collapses rows → result per **column**
- `axis=1` → collapses columns → result per **row**
- No axis → operates on ALL elements → scalar result

## Indexing/Slicing Rules
- Stop is **excluded** in slicing (like Python)
- `[::-1]` reverses the array
- Boolean conditions need `&`/`|` with **parentheses** around each condition
- Slicing creates a **view** — changes propagate!

## Function Behavior
- `astype()` truncates, does NOT round: `4.9 → 4`
- `astype()` returns a new array — does NOT modify original
- `reshape()` returns new array — does NOT modify original
- `shuffle()` modifies in place — returns **None**
- `flatten()` → copy (safe) | `ravel()` → view (risky)

## Broadcasting Rules
- Compare shapes from the **right**
- Compatible if equal or one is 1
- Missing dimension = 1
- `(4,3) + (4,)` → **ERROR** (3≠4 from right)

## Data Types
- int + float → float64
- anything + string → string
- `np.zeros()` default → float64
- `np.arange()` with ints → int64
- `np.linspace()` → always float64

## Random Module
- `randint(low, high)` → high excluded
- `rand()` → [0, 1) — 1 not included
- `seed()` → same seed = same numbers
- `shuffle()` → in place, returns None

## Linear Algebra
- `*` = element-wise | `@` = matrix multiplication
- `A @ B` requires inner dimensions match
- Matrix multiplication is NOT commutative
- `.T` transposes: `(m,n)` → `(n,m)`

---

# 🔥 TOP MCQ TRAPS

> **1.** `np.arange(1, 5)` → `[1 2 3 4]` — stop value 5 is **excluded**.

> **2.** `np.linspace(0, 1, 5)` → includes both 0 and 1 — stop is **included**.

> **3.** `np.array([1, 4, 4.3, 4.9, 4.8], dtype='int32')` → `[1 4 4 4 4]` — truncation, NOT rounding.

> **4.** `(5,)` is a 1D array shape. `(5,1)` is a 2D column. They are NOT the same.

> **5.** `np.zeros(5)` → `[0. 0. 0. 0. 0.]` — default dtype is float64, shows decimals.

> **6.** `axis=0` = column-wise result (collapses rows). `axis=1` = row-wise result.

> **7.** `len()` on 2D array = number of ROWS only, NOT total elements.

> **8.** `astype()` returns a NEW array. Original is unchanged.

> **9.** `reshape()` does NOT modify the array. Must assign result.

> **10.** `np.random.shuffle()` returns `None`. It modifies in place.

> **11.** `flatten()` → copy (safe). `ravel()` → view (changes propagate).

> **12.** `np.vstack((A, B))` needs double brackets. `np.vstack(A, B)` → TypeError.

> **13.** `hstack` on 1D arrays → concatenation `(8,)`, NOT a 2-column table `(4,2)`.

> **14.** Boolean conditions: `arr[(arr > 5) & (arr < 10)]` — parentheses required!

> **15.** `(4,3) + (4,)` → Broadcasting ERROR. Compare from right: 3 ≠ 4.

> **16.** `(4,3) + (3,)` → WORKS. (3,) is treated as (1,3), 1 stretches to 4.

> **17.** Slicing creates a **view**. `arr[2:6][0] = 999` changes the original array.

> **18.** `np.where(marks > 50)` with only condition → returns a **tuple**, use `[0]`.

> **19.** `np.where(cond, "Pass", 0)` → everything becomes string: `'Pass'` and `'0'`.

> **20.** `np.median()` is a function: `np.median(arr)` ✅. `arr.median()` ❌.

> **21.** Matrix multiplication: `(2,3) @ (3,2)` = `(2,2)`. Inner dims must match.

> **22.** `A @ B ≠ B @ A` — matrix multiplication is NOT commutative.

> **23.** `randint(10, 20)` → range is 10 to 19. High value 20 is excluded.

> **24.** `np.eye(3)` → dtype is `float64`. Use `dtype=int` for integer output.

> **25.** `reshape(-1, -1)` → Error. Only ONE dimension can be -1.

> **26.** `col (3,1) + row (3,)` → result is `(3,3)`, not `(3,)`. Grid, not vector!

> **27.** `.T` is a property, not a method: `arr.T` ✅, `arr.T()` ❌.

> **28.** `np.arange(1, 10)` gives `int64`, `np.linspace(1, 9, 9)` gives `float64`.

> **29.** `np.delete()`, `np.insert()`, `np.append()` all return NEW arrays. Original unchanged.

> **30.** `np.diag(matrix)` extracts diagonal. `np.diag(1d_array)` creates diagonal matrix. Same function, two roles.
