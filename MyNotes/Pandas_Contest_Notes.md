# 🎯 Pandas Contest Notes — Complete Revision

> **Source:** Every concept, example, and question below comes directly from the teacher's repository.
> **Goal:** If the teacher asks an MCQ about anything they taught in Pandas, these notes should give you the best possible chance of answering it correctly.

---

# 1. What is Pandas?

### What?
Pandas = **NumPy + labels**. It adds row/column names to NumPy arrays, making data analysis intuitive.

### Why?
- NumPy arrays use position numbers only (0, 1, 2…). Pandas adds **names** (column headers, row labels).
- Built for real-world tabular data (CSV, Excel, databases).

### Import
```python
import pandas as pd
import numpy as np
```

### 🧠 MCQ Facts
- `pd.Series` = 1D data with an index (like a labelled array)
- `pd.DataFrame` = 2D table with row index + column names
- Pandas is built **on top of** NumPy

---

# 2. Series

### What?
A **1D labelled array**. Every value has an index.

```python
total_bill = [4700, 8000, 1200, 500]
s = pd.Series(total_bill, index=['mon', 'tue', 'wed', 'fri'])
print(s)
```
```text
mon    4700
tue    8000
wed    1200
fri     500
dtype: int64
```

```python
print(type(s))   # <class 'pandas.core.series.Series'>
```

### 🧠 MCQ Facts
- Selecting a single column from a DataFrame gives you a **Series**
- `df['column_name']` → Series
- `df[['column_name']]` → DataFrame (with one column)

### 🎯 Remember
> **Single brackets → Series. Double brackets → DataFrame.**

---

# 3. DataFrame

### What?
A **2D table** — rows and columns, each with labels.

### Creating from a dictionary

```python
restaurant_data = {
    'Table_No': [12, 14, 15],
    'Bill_Amount': [1250, 890, 2400],
    'Waiter_Name': ['Amit', 'Priya', 'Rahul']
}
df = pd.DataFrame(restaurant_data)
```

```python
print(df['Bill_Amount'])        # Series
print(type(df['Bill_Amount']))  # <class 'pandas.core.series.Series'>
```

### `.values` / `.to_numpy()` — The NumPy array underneath

```python
print(df.values)
print(type(df.values))   # <class 'numpy.ndarray'>
```

### 🧠 MCQ Facts
- Dictionary keys become **column names**
- Dictionary values (lists) become **column data**
- All lists must be the **same length**

---

# 4. Loading & Inspecting Data

## 4.1 `pd.read_csv()`

```python
df = pd.read_csv("tip.csv")
```

## 4.2 Inspection Functions

| Function | What it shows |
|---|---|
| `df.head(n)` | First n rows (default 5) |
| `df.tail(n)` | Last n rows (default 5) |
| `df.sample(n)` | n random rows |
| `df.shape` | (rows, columns) tuple |
| `df.columns` | List of column names |
| `df.info()` | Dtypes + null counts — **one-stop shop** |
| `df.describe()` | Statistics of **numeric** columns only |
| `df.dtypes` | Data type of each column |

### `info()` vs `describe()`

```python
df.info()
```
```text
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 244 entries, 0 to 243
Data columns (total 7 columns):
 #   Column      Non-Null Count  Dtype
---  ------      --------------  -----
 0   total_bill  244 non-null    float64
 1   tip         244 non-null    float64
 2   sex         244 non-null    object
 3   smoker      244 non-null    object
 4   day         244 non-null    object
 5   time        244 non-null    object
 6   size        244 non-null    int64
```

```python
df.describe()  # Only shows total_bill, tip, size — the numeric columns
```

> ⚠️ **MCQ TRAP**: `describe()` **skips non-numeric columns** (object dtype). It shows fewer columns than `info()`.

## 4.3 `value_counts()`, `unique()`, `nunique()`

```python
print(df['day'].value_counts())   # count per unique value (sorted desc)
print(df['day'].unique())         # array of unique values
print(df['day'].nunique())        # number of unique values
```
```text
day
Sat     87
Sun     76
Thur    62
Fri     19
Name: count, dtype: int64

['Sun' 'Sat' 'Thur' 'Fri']
4
```

### 🧠 MCQ Facts
- `value_counts()` returns a **Series**, sorted by count (largest first)
- `unique()` returns a **numpy array** — NOT sorted
- `nunique()` returns a single **integer**
- `value_counts(normalize=True)` → percentages instead of counts

### ML Perspective
- `info()` → check for missing values before training
- `describe()` → check data distribution, find outliers (compare mean vs median)
- `value_counts()` → check class imbalance

---

# 5. Selecting Columns

### Single column → Series
```python
df['tip']           # returns a Series
```

### Multiple columns → DataFrame
```python
df[['day', 'tip']]  # returns a DataFrame
```

### New column
```python
df['tip_percent'] = round(df['tip'] / df['total_bill'] * 100, 2)
```

### ⚖️ Comparison: Single vs Double Brackets

| Syntax | Returns | Type |
|---|---|---|
| `df['tip']` | One column | **Series** |
| `df[['tip']]` | One column in table form | **DataFrame** |
| `df[['day', 'tip']]` | Multiple columns | **DataFrame** |

> ⚠️ **MCQ TRAP**: `df['tip']` and `df[['tip']]` look similar but return different types!

---

# 6. `loc` vs `iloc` — ⭐ THE MOST IMPORTANT TOPIC

## 6.1 `iloc` — Integer Location (position-based)

```python
df.iloc[9]              # 10th row (0-indexed)
df.iloc[-3:]            # last 3 rows
df.iloc[:, :2]          # all rows, first 2 columns
df.iloc[9, 0]           # 10th row, 1st column value
```

## 6.2 `loc` — Label-based

```python
df.loc[20:25]                        # rows with LABELS 20 to 25
df.loc[20:25, 'sex':'size']          # rows 20–25, columns sex through size
df.loc[df['tip'] > 5]               # filter with condition
df.loc[df['tip'] > 5, 'total_bill'] # filter + select column
```

## ⚖️ THE Critical Difference: `loc` vs `iloc`

| | `loc` | `iloc` |
|---|---|---|
| Selects by | **Label** (name) | **Position** (number) |
| Slice end | **INCLUDED** ✅ | **EXCLUDED** ❌ |
| Columns | By **name** | By **number** |
| Accepts condition | **Yes** | No |
| `[10:20]` gives | **11 rows** (10,11,12…20) | **10 rows** (10,11,12…19) |

> 🔥 **IMPORTANT — This is the single most common Pandas mistake:**

```python
df.loc[10:20]    # 11 rows → labels 10, 11, 12, ..., 20 (INCLUSIVE)
df.iloc[10:20]   # 10 rows → positions 10, 11, 12, ..., 19 (EXCLUSIVE)
```

### 🧠 MCQ Facts
- `loc` end is **included** (unlike Python slicing!)
- `iloc` end is **excluded** (like Python slicing)
- `loc` can slice columns by name: `df.loc[5:10, 'total_bill':'time']`
- `loc` accepts boolean conditions: `df.loc[df['age'] > 30]`
- `iloc` does NOT accept boolean conditions

### Using `loc` to modify values

```python
df['service'] = 'normal'
df.loc[df['tip_percent'] > 20, 'service'] = 'generous'
```

### The post-filter trap

```python
sat = df[df['day'] == 'Sat']
sat.iloc[0]    # ✅ works — first Saturday bill
sat.loc[0]     # ❌ KeyError! — label 0 doesn't exist, first Sat is label 19
```

> ⚠️ **MCQ TRAP**: After filtering, original labels are kept. The first row of `sat` might be labeled 19, not 0. `iloc[0]` works (position), `loc[0]` fails (no label 0).

### Fix: `reset_index(drop=True)`
```python
sat = sat.reset_index(drop=True)
sat.loc[0]     # ✅ now works
```

### 🎯 Remember
> **loc = labels, inclusive end. iloc = integers, exclusive end.**

---

# 7. Boolean Indexing / Filtering

## 7.1 Single condition

```python
df[df['total_bill'] > 30]
```

## 7.2 Multiple conditions — `&` (AND), `|` (OR), `~` (NOT)

```python
# AND — both conditions true
df[(df['size'] >= 3) & (df['tip'] > 4)]

# OR — either condition true
df[(df['day'] == 'Sat') | (df['day'] == 'Sun')]

# NOT — invert
df[~(df['time'] == 'Dinner')]   # NOT dinner = Lunch
```

> ⚠️ **MCQ TRAP — Parentheses are MANDATORY**

```python
df[df['size'] > 4 & df['tip'] > 5]     # ❌ Error!
df[(df['size'] > 4) & (df['tip'] > 5)]  # ✅ Correct
```
**`&` has higher precedence than `>`, so without parentheses Python evaluates `4 & df['tip']` first.**

### Counting filtered results

```python
df[df['Pclass'] == 1].shape[0]   # number of first-class passengers
```

### Complex Titanic examples (from repo)

```python
# Men travelling alone
t[(t['Sex']=='male') & (t['SibSp']==0) & (t['Parch']==0)].shape[0]

# Children under 10 in third class
t[(t['Age'] < 10) & (t['Pclass'] == 3)].shape[0]

# Men over 40 who survived
t[(t['Sex'] == 'male') & (t['Age'] > 40) & (t['Survived'] == 1)].shape[0]
```

### Percentage calculation pattern
```python
# Survival rate of women
survived = t[(t['Sex'] == 'female') & (t['Survived'] == 1)].shape[0]
total = t[t['Sex'] == 'female'].shape[0]
pct = survived / total * 100
```

---

# 8. Missing Values — `NaN`

## 8.1 Finding Blanks

```python
df.isnull().sum()           # blanks per column
df['Age'].isnull().sum()    # blanks in one column
df.isna().sum()             # same as isnull() — two names, one function
df.notnull()                # opposite of isnull()
```

### Titanic missing values:
```text
Age        177
Cabin      687
Embarked     2
```

> 🔥 **IMPORTANT — The Silent Bug**

```python
# 150 passengers over 40 + 564 aged 40 or under = 714, NOT 891!
# 177 passengers silently dropped from every Age filter
# No error, no warning — your answer is WRONG and looks RIGHT
```

> ⚠️ **MCQ TRAP**: `np.nan == np.nan` is **False**! NaN is not equal to itself.

### 🧠 MCQ Facts
- `isnull()` and `isna()` are **identical** — two names for one function
- `notnull()` and `notna()` are **identical** — the opposite
- Pandas **silently skips NaN** in aggregations (mean, sum, etc.)
- `mean()` divides by non-null count, NOT total row count

## 8.2 Deleting — `dropna()`

```python
df.dropna()                      # drop ANY row with ANY blank → aggressive!
df.dropna(subset=['Age'])        # drop only where Age is blank
df.dropna(subset=['Embarked'])   # drop only where Embarked is blank
df.dropna(axis=1)                # drop COLUMNS that have blanks
```

### Planets dataset example (from repo):
| Command | Rows/Cols remaining |
|---|---|
| `p.dropna()` | 498 of 1035 rows |
| `p.dropna(subset=['mass'])` | 513 rows |
| `p.dropna(subset=['distance'])` | 808 rows |
| `p.dropna(axis=1)` | 3 columns only (method, number, year) |

> ⚠️ **MCQ TRAP**: `dropna()` without `subset` = **any blank anywhere kills the row**. This can destroy half your data!

### 🧠 MCQ Facts
- `dropna()` returns a **new** DataFrame — does NOT modify original
- Use `inplace=True` to modify original
- `axis=0` (default) → drop **rows** | `axis=1` → drop **columns**

## 8.3 Filling — `fillna()`

```python
df['mass'].fillna(0)                    # fill with a fixed value
df['mass'].fillna(df['mass'].mean())    # fill with the average
df['mass'].fillna(df['mass'].median())  # fill with the middle value
```

### When to use mean vs median (from repo):

| | Mean | Median |
|---|---|---|
| Fare (mean=32.20, median=14.45) | ❌ Skewed by outliers | ✅ Use median |
| Age (mean≈30, median≈28) | OK for symmetric | ✅ Safer choice |

> ⚠️ **MCQ TRAP — Filling changes statistics!**

| | Before fill | After mean-fill |
|---|---|---|
| mean | 2.64 | 2.64 (unchanged) |
| **median** | **1.26** | **2.64** (shifted!) |
| **std** | **3.82** | **2.69** (shrunk!) |

**522 identical values landed in the column → median moved, std shrunk.**

### ⚖️ Comparison: `fillna()` vs `dropna()`

| | `fillna()` | `dropna()` |
|---|---|---|
| What it does | Replaces NaN with a value | Removes rows/cols with NaN |
| Data loss | None — all rows kept | Potentially massive |
| Risk | Introduces fake data | Loses real data |
| When to use | You can justify the fill value | Blanks are few and random |

### 🎯 Remember
> Fill with **median** when data is skewed (mean ≠ median). Fill with **mean** when data is symmetric.

---

# 9. Duplicates

```python
df.duplicated()                     # True for rows that repeat earlier ones
df.duplicated().sum()               # count of duplicate rows
df.drop_duplicates()                # remove exact duplicates
df.duplicated(subset=['method'])    # "same" only in named columns
```

> ⚠️ **MCQ TRAP**: `subset=` changes the meaning completely!
> - Without subset → entire row must be identical
> - With `subset=['method']` → only the method column matters

### 🧠 MCQ Facts
- `duplicated()` keeps the **first** occurrence, marks rest as True
- `drop_duplicates()` returns a new DataFrame — original unchanged
- `ignore_index=True` → resets index after dropping

---

# 10. Saving Data

```python
df.to_csv("output.csv", index=False)   # save without row numbers
```

> ⚠️ **MCQ TRAP — `index=False` with grouped data**

After `groupby`, the group key becomes the **index**. If you save with `index=False`, **you lose the group labels!**

```python
summary = df.groupby('day').agg(avg_tip=('tip', 'mean'))
summary.to_csv("report.csv")              # day is in the index → saved correctly
summary.to_csv("report.csv", index=False)  # day is LOST!

# Safe pattern:
summary.reset_index().to_csv("report.csv", index=False)  # ✅
```

### 🎯 Remember
> After groupby: **`reset_index()` first, then `to_csv(index=False)`**.

---

# 11. Adding / Modifying Columns

## Creating new columns

```python
df['tip_percent'] = round(df['tip'] / df['total_bill'] * 100, 2)
df['tip_pct'] = df['tip'] / df['total_bill'] * 100
```

## Conditional column with `loc`

```python
df['service'] = 'normal'
df.loc[df['tip_percent'] > 20, 'service'] = 'generous'

# Titanic example
t['travel_group'] = 'alone'
t.loc[(t['SibSp'] + t['Parch'] > 0), 'travel_group'] = 'With family'

# Age group
t['age_group'] = 'adult'
t.loc[t['Age'] < 18, 'age_group'] = 'child'
```

> ⚠️ **MCQ TRAP**: `t.loc[t['Age'] < 18, 'age_group'] = 'child'` — 177 passengers with NaN age will stay as 'adult' because `NaN < 18` is `False`.

---

# 12. Sorting

## `sort_values()`

```python
df.sort_values('total_bill', ascending=False)   # largest first
df.nlargest(5, 'tip')                           # top 5 tips
```

## `sort_values()` vs `sort_index()`

| | `sort_values()` | `sort_index()` |
|---|---|---|
| Sorts by | **Column values** | **Index labels** |
| Main use | Find top/bottom records | Reorder after groupby |

---

# 13. Bonus Selection Methods (from repo)

## `isin()` — instead of chaining `|`

```python
# Long way:
df[(df['day'] == 'Sat') | (df['day'] == 'Sun')]

# Short way:
df[df['day'].isin(['Sat', 'Sun'])]
```

## `between()` — instead of two conditions with `&`

```python
# Long way:
df[(df['total_bill'] > 20) & (df['total_bill'] < 30)]

# Short way:
df[df['total_bill'].between(20, 30)]   # inclusive by default!
```

> ⚠️ **MCQ TRAP**: `between(20, 30)` is **inclusive** on both sides: `20 ≤ x ≤ 30`

## `set_index()` and `reset_index()`

```python
d2 = df.set_index('day')     # 'day' becomes the index
d2.loc['Sun']                # now you can select by day name!
d2.reset_index()             # pushes index back to column
```

---

# 14. `groupby` — ⭐ CORE TOPIC

## 14.1 The 3-Step Recipe

```python
df.groupby('day')['tip'].mean()
#      1. Group   2. Pick  3. Apply
```

| Step | What | Which column |
|---|---|---|
| **1 Group** | `.groupby('day')` | A **category** column |
| **2 Pick** | `['tip']` | A **number** column |
| **3 Apply** | `.mean()` | `mean sum min max count median std` |

### From the repo:

```python
tip.groupby('day')['tip'].mean().round(2)
```
```text
day
Fri     2.73
Sat     2.99
Sun     3.26
Thur    2.77
```

## 14.2 Pick BEFORE Aggregate — Not After!

```python
df.groupby('day').mean()['tip']      # ❌ crashes (tries to average strings)
df.groupby('day')['tip'].mean()      # ✅ works — picks first, then aggregates
```

> ⚠️ **MCQ TRAP**: Always **pick the column** before applying the aggregation function.

## 14.3 `groupby().size()` — count rows per group

```python
tip.groupby('day').size()
```
```text
Fri     19
Sat     87
Sun     76
Thur    62
```

## 14.4 Groupby on Titanic (from repo)

```python
# Survival rate per class
titanic.groupby('Pclass')['Survived'].mean().round(2) * 100
# 1st: 63%, 2nd: 47%, 3rd: 24% — "somewhat safer" is dishonest!

# Average fare per class
titanic.groupby('Pclass')['Fare'].mean()
```

---

# 15. Named Aggregation with `.agg()` — ⭐

## 15.1 Syntax

```python
df.groupby('key').agg(
    column_name_you_pick = ('column_to_use', 'function_to_apply'),
    ...
)
```

### From the repo:

```python
tips.groupby("day").agg(
    Bill_count=("total_bill", 'size'),
    avg_bill=("total_bill", 'mean'),
    avg_tip=('tip', 'mean')
)
```

### Available function strings:
`'size'`, `'count'`, `'sum'`, `'mean'`, `'median'`, `'min'`, `'max'`, `'std'`, `'nunique'`, `'first'`, `'last'`

> ⚠️ **MCQ TRAP — Tuple order matters!**

```python
('total_bill', 'mean')   # ✅ correct: (column, function)
('mean', 'total_bill')   # ❌ wrong: looks for a column called 'mean'
```

## 15.2 `size` vs `count` — The Silent Trap

| Function | What it counts |
|---|---|
| `size` | **All rows** — blanks included |
| `count` | **Non-missing values** — blanks excluded |

```text
If no blanks → size == count (you never notice)
If blanks exist → size > count (your denominator is wrong!)
```

> 🔥 **IMPORTANT**: When you report a group mean, check if `size` and `count` differ. If they do, the mean was computed from fewer values than you think.

### From the repo (bookings example):
```text
12 bookings. 8 party sizes recorded.
Average table size = 3.62 — computed from 8, not 12!
```

### 🎯 Remember
> **size = rows (including blanks). count = non-null values only.** Always check both.

---

# 16. `reset_index()` — After Groupby

## The Problem

After `groupby`, the group key becomes the **index**, not a column.

```python
summary = tips.groupby("day").agg(avg_tip=('tip', 'mean'))
print(summary.columns.tolist())  # ['avg_tip'] — 'day' is NOT here!
print(summary.index.tolist())    # ['Fri', 'Sat', 'Sun', 'Thur']

summary[summary["day"] == "Sun"]  # ❌ KeyError: 'day'
```

## The Fix

```python
flat = summary.reset_index()
print(flat.columns.tolist())     # ['day', 'avg_tip'] — day is back!
flat[flat["day"] == "Sun"]       # ✅ works
```

### 🎯 Remember
> **Rule of thumb:** If the next step is filter, merge, plot, or save → `reset_index()` first.

---

# 17. `idxmax()` and `idxmin()`

### What?
Returns the **index label** of the row with the max/min value — not the value itself.

### The two-step pattern (from repo):

```python
rows = tips.groupby('day')['tip'].idxmax()   # step 1 — which rows
tips.loc[rows]                                # step 2 — go get them
```

### From the repo:
```python
tips.loc[170]   # Row 170 → the Saturday table with the $10 tip on a $50.81 bill
```

### 🧠 MCQ Facts
- `idxmax()` returns the **index label**, NOT the value
- Feed the result into `.loc[]` to get the full row
- On a tie → returns only the **first** match
- If every value in a group is NaN → raises an Error

---

# 18. Multi-Column Groupby & `.unstack()`

## Groupby with two columns (from Assignment 3, Q9)

```python
# Average mpg for every combination of origin and cylinders
cars.groupby(['origin', 'cylinders'])['mpg'].mean()
```

## `.unstack()` — Pivot a grouped result

```python
# Reshape: origins as rows, cylinder counts as columns
cars.groupby(['origin', 'cylinders'])['mpg'].mean().unstack()
```

This turns the inner group level into **columns**, creating a pivot-table-like view.

### 🧠 MCQ Facts
- `unstack()` takes the **innermost** index level and makes it column headers
- NaN appears where a combination doesn't exist (e.g., Japanese cars with 8 cylinders)
- Opposite of `unstack()` is `stack()`

---

# 19. String Methods — The `.str` Accessor

## 18.1 Why?

Text data is messy: `'ravi sharma'`, `'RAVI SHARMA'`, `'  Ravi Sharma '` are the **same person** but pandas treats them as three different strings.

## 18.2 Key Methods

| Method | What it does |
|---|---|
| `.str.lower()` | All lowercase |
| `.str.upper()` | All uppercase |
| `.str.strip()` | Remove whitespace from **ends only** |
| `.str.split()` | Break into parts (by whitespace) |
| `.str.split().str[i]` | Get the i-th part |
| `.str.split().str.join(' ')` | Collapse internal whitespace |
| `.str.contains(x, na=False)` | Boolean mask for filtering |
| `.str.replace(a, b)` | Swap a substring |
| `.str.startswith(x)` | Boolean: starts with x? |
| `.str.endswith(x)` | Boolean: ends with x? |

## 18.3 The Full Cleaning Pipeline (from repo)

```python
bookings['customer'] = bookings['customer'].str.lower() \
                        .str.strip() \
                        .str.split().str.join(" ")
```

This fixed 12 "unique" customers down to **5 real customers**.

### Steps:
1. `.str.lower()` → case-fold (`RAVI SHARMA` → `ravi sharma`)
2. `.str.strip()` → trim ends (`  ravi sharma ` → `ravi sharma`)
3. `.str.split().str.join(" ")` → collapse double spaces (`priya  iyer` → `priya iyer`)

> ⚠️ **MCQ TRAP**: `.strip()` only removes whitespace from the **ends** — it does NOT fix double spaces in the middle!

### Extracting parts

```python
# Extract first name (brand from car name)
cars['brand'] = cars['name'].str.split().str[0]
```

---

# 19. Data Type Conversion — `astype()`

```python
# Convert string numbers to integers
old = np.array([['62', '71', '55', '80'], ['49', '58', '66', '73']])
old = old.astype('int32')
print(np.sum(old, axis=1))   # [268 246]

# Convert to category (saves memory)
cars['origin'] = cars['origin'].astype('category')
```

### ML Perspective
`astype('category')` is useful for reducing memory on columns with few unique values.

---

# 20. Combining DataFrames

## `pd.concat()` — Stacking DataFrames

```python
# Vertical stack (add rows)
combined = pd.concat([df1, df2], axis=0)

# Horizontal stack (add columns)
combined = pd.concat([df1, df2], axis=1)
```

### From the repo (NumPy equivalent):
```python
# Using NumPy vstack equivalent in Pandas
table = np.vstack((table, old))   # combine student records
```

---

# 21. Plotting for ML (from repo)

The plotting notebook covers these key concepts that may appear in MCQs:

### Key Facts:
- A model that always predicts "died" on Titanic scores **61.6%** accuracy — any model must beat this baseline
- `Fare` mean=32.20, median=14.45 → **skewed right** → use median to fill
- `Fare` has **116 outliers** (beyond box plot whiskers)
- `Cabin` (687 blanks out of 891) separates survivors better than almost anything: **66.7% vs 30.0%** survival rate

### Matplotlib/Seaborn basics:
```python
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(7, 4))
sns.histplot(data=df, x='Fare')
plt.title('Fare Distribution')
plt.savefig('name.png')    # MUST come before plt.show()!
plt.show()
```

> ⚠️ **MCQ TRAP**: `plt.savefig()` must come **before** `plt.show()` — after `show()`, the figure is gone.

---

# 📝 Repository Questions with Answers

## From Pandas_Basics_2 (Exit Quiz)

**Q1.** `df.loc[10:20]` gives how many rows? And `df.iloc[10:20]`?
> **Answer:** `loc[10:20]` → **11 rows** (inclusive). `iloc[10:20]` → **10 rows** (exclusive).

**Q2.** Fix: `df[df['size'] > 4 & df['tip'] > 5]`
> **Answer:** `df[(df['size'] > 4) & (df['tip'] > 5)]` — add parentheses!

**Q3.** Filtered `sat = df[df['day'] == 'Sat']`. Now `sat.loc[0]` fails. Why?
> **Answer:** Filtering keeps original labels. First Saturday is labeled 19, not 0. Use `iloc[0]` or `reset_index()`.

**Q4.** One line: which day had the most bills?
> **Answer:** `df['day'].value_counts().idxmax()` → `'Sat'`

## From Pandas Assignment 2

**A3.** `df.loc[20:25]` — how many rows and why not 5?
> **Answer:** **6 rows** (20, 21, 22, 23, 24, 25). `loc` end is inclusive!

**B5.** Age group: passengers with NaN age get classified as 'adult'.
> **Answer:** 177 passengers with no age → all labeled 'adult' since `NaN < 18` is `False`.

## From Pandas Assignment 3 (mpg dataset)

**Q8.** `size` vs `count` difference.
> **Answer:** `size` counts all rows including blanks. `count` counts non-null only. They differ when there are missing values.

**Q10.** Japanese cars average 30.5 mpg vs American 20.1 — is the conclusion correct?
> **Answer:** No. American cars are mostly 6-8 cylinder; Japanese are mostly 4-cylinder. Compare like with like (same cylinders).

**Q12.** `astype('category')` saves memory.

**Q15.** String cleaning: extract brand from car name using `.str.split().str[0]`.

## From Data Cleaning Notebook

**T1.** Three damaged columns: Age (177), Cabin (687), Embarked (2).

**T2.** `t.dropna()` without arguments: only **183** of 891 passengers survive. Cabin caused most damage.

**Key insight:** `has_cabin` column → 66.7% survival rate WITH cabin vs 30.0% WITHOUT.

## From Groupby Notebooks

**Key finding:** Tip percentage drops as party size increases (21.7% for 1 person → 14.1% for 5 people). This is why restaurants add automatic service charge for large groups.

**Q: Pick before aggregate, not after.**
```python
df.groupby('day').mean()['tip']      # ❌ crashes
df.groupby('day')['tip'].mean()      # ✅ correct
```

---

# 🚨 LAST-MINUTE MCQ REVISION

## Series vs DataFrame
- `df['col']` → **Series** | `df[['col']]` → **DataFrame**
- Series has one column, DataFrame has one or more

## Indexing Rules
- `loc` = **labels**, end **included**
- `iloc` = **integers**, end **excluded**
- `df.loc[10:20]` → 11 rows | `df.iloc[10:20]` → 10 rows

## Boolean Filtering
- Multiple conditions: each in **parentheses**, joined with `&` / `|`
- `~` for NOT
- `NaN` comparisons always return `False`

## Missing Values
- `isnull()` = `isna()` — same function
- `notnull()` = `notna()` — same function
- Pandas **silently skips NaN** in aggregations
- `np.nan == np.nan` → **False**
- `dropna()` without subset → any NaN kills the row
- `fillna()` with mean → median shifts, std shrinks

## `groupby`
- Always pick column BEFORE aggregating
- After groupby → group key becomes **index**, not column
- Use `reset_index()` to get it back
- `size` counts all rows, `count` skips NaN

## `describe()` and `info()`
- `describe()` → numeric columns only
- `info()` → all columns with dtypes and null counts

## `value_counts()`
- Returns Series sorted by count (desc)
- `normalize=True` → proportions instead of counts
- `idxmax()` on value_counts → most common value

## String Methods
- `.str.strip()` → ends only, NOT middle
- `.str.split().str.join(' ')` → fixes internal double spaces
- Always `.str.lower()` before comparing text

## Saving
- After groupby: `reset_index()` → then `to_csv(index=False)`
- `index=False` on grouped data without reset → **loses group labels**

## Plotting
- `plt.savefig()` BEFORE `plt.show()`
- Skewed data (mean >> median) → use median for fill

## Data Type Gotchas
- `object` dtype = text/mixed data
- `float64` for decimals, `int64` for whole numbers
- `describe()` skips `object` columns

---

# 🔥 TOP MCQ TRAPS

> **1.** `df.loc[10:20]` → **11 rows** (inclusive). `df.iloc[10:20]` → **10 rows** (exclusive).

> **2.** `df['tip']` → **Series**. `df[['tip']]` → **DataFrame**. Different types!

> **3.** `df[df['size'] > 4 & df['tip'] > 5]` → Error! Need parentheses: `df[(df['size'] > 4) & (df['tip'] > 5)]`

> **4.** After filtering `sat = df[df['day'] == 'Sat']`, `sat.loc[0]` **fails** because original labels are kept. Use `iloc[0]`.

> **5.** `describe()` shows **only numeric columns**. `info()` shows **all columns**.

> **6.** `np.nan == np.nan` → **False**. NaN is never equal to itself.

> **7.** `dropna()` without `subset` → any NaN in any column kills the entire row. Can lose 50%+ of data.

> **8.** `isnull()` and `isna()` are the **same function** with two names.

> **9.** `groupby` makes the group key the **index**, not a column. `summary['day']` → KeyError!

> **10.** Fix after groupby: `summary.reset_index()` puts the key back as a column.

> **11.** `size` counts **all rows** (including NaN). `count` counts **non-null only**.

> **12.** `fillna(mean)` preserves the mean but **shifts the median** and **shrinks std**.

> **13.** `value_counts()` returns values sorted by **frequency** (most common first).

> **14.** `.str.strip()` removes whitespace from **ends only** — NOT from the middle.

> **15.** `to_csv(index=False)` after groupby → **loses group labels**! Use `reset_index()` first.

> **16.** Pick column BEFORE aggregate: `df.groupby('day')['tip'].mean()` ✅, not `df.groupby('day').mean()['tip']` ❌.

> **17.** `loc` can take a boolean condition: `df.loc[df['age'] > 30]` ✅. `iloc` **cannot**.

> **18.** `idxmax()` returns the **index label**, not the maximum value itself.

> **19.** `unique()` returns a **NumPy array** (unordered). `nunique()` returns a single **integer**.

> **20.** `between(20, 30)` is **inclusive** on both sides: 20 ≤ x ≤ 30.

> **21.** `NaN < 18` → **False**. So `df.loc[df['Age'] < 18]` silently excludes all NaN-age rows.

> **22.** `plt.savefig()` must come **before** `plt.show()` — after show, figure is cleared.

> **23.** Pandas uses `ddof=1` for `std()` (sample std). NumPy uses `ddof=0` (population std). Different defaults!

> **24.** `agg()` tuple order: `('column', 'function')` ✅ — not `('function', 'column')` ❌.

> **25.** A model predicting "died" for all Titanic passengers = **61.6% accuracy**. Any useful model must beat this.

> **26.** `value_counts(normalize=True)` → proportions (0–1), not percentages (0–100).

> **27.** `drop_duplicates(subset=['col'])` → "same" means same in **that column only**, not entire row.

> **28.** `has_cabin` survival: **66.7% with cabin, 30.0% without** — most damaged column is most predictive.

> **29.** `groupby('day').mean()` crashes if DataFrame has string columns. Pick numeric column first.

> **30.** `reset_index(drop=True)` → discards old index. `reset_index()` without `drop` → keeps old index as a new column.
