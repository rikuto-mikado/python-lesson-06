# Python Conventions

## `_` for Unused Variables

Use `_` when a value must be received but is intentionally not used.

```python
# iterrows() returns (index, row) — index is not needed
for _, row in df.iterrows():
    print(row['name'])

# factorize() returns (codes, uniques) — uniques is not needed
codes, _ = pandas.factorize(df['userId'])

# function returns multiple values — second is not needed
first, _ = some_function()
```

This is a widely accepted convention. It signals to other readers:
> "I know this value exists. I'm deliberately ignoring it."

---

## Type Casting from NumPy to Python

NumPy arrays use their own types (`np.int32`, `np.float64`). When printing or passing values to non-NumPy code, cast to plain Python types.

| NumPy type | Cast to | How |
|---|---|---|
| `np.int32`, `np.int64` | `int` | `int(x)` |
| `np.float32`, `np.float64` | `float` | `float(x)` |
| numpy array | Python list | `arr.tolist()` |

```python
# Without casting — verbose output
x = np.int32(5)
print(x)  # np.int32(5)

# With casting — clean output
print(int(x))  # 5
```

Common pattern when building a list of tuples from a sparse matrix:

```python
sorted((int(u), int(i), float(r)) for u, i, r in all_ratings)
```

---

## Unpacking with `*` (splat)

Capture remaining values into a list when you only need the first/last.

```python
first, *rest = [1, 2, 3, 4]
# first = 1, rest = [2, 3, 4]

*rest, last = [1, 2, 3, 4]
# rest = [1, 2, 3], last = 4
```
