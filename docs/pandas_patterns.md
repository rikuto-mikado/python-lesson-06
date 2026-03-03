# Pandas Patterns

## Loading Data

```python
import pandas

df = pandas.read_csv("file.csv")
df = pandas.read_csv("file.csv")[["col1", "col2"]]  # load only specific columns
df.head()     # first 5 rows
df.tail()     # last 5 rows
df.shape      # (n_rows, n_cols)
df.dtypes     # column data types
df.describe() # basic statistics
```

---

## Iterating Over Rows

### `iterrows()` — simple but slow

```python
# Returns (index, row) pairs.
# Use _ if the index is not needed.
for _, row in df.iterrows():
    print(row['userId'], row['rating'])
```

Avoid for large DataFrames — it's slow. Prefer vectorized operations when possible.

### Vectorized alternative (faster)

```python
# Instead of looping, operate on entire columns at once
df['rating'] * 2          # multiply all ratings by 2
df['userId'] - 1          # shift all user IDs
df[df['rating'] >= 4]     # filter rows
```

---

## Assigning Sequential Inner IDs

### Manual (order of first appearance)

```python
id_map = {}
for raw_id in df['userId']:
    if raw_id not in id_map:
        id_map[raw_id] = len(id_map)
```

### With `factorize()` (concise, same result)

```python
# Returns (codes, unique_values)
# codes: integer array, 0-based, assigned in order of first appearance
codes, _ = pandas.factorize(df['userId'])
```

`factorize()` is the idiomatic pandas way. Use it when you need to map arbitrary IDs (e.g., userId=472) to sequential integers (0, 1, 2, ...).

---

## Filtering

```python
df[df['rating'] >= 4]                        # single condition
df[(df['rating'] >= 4) & (df['userId'] == 1)] # multiple conditions (use & not and)
df[df['movieId'].isin([1, 2, 3])]            # membership filter
```

---

## Grouping

```python
# Average rating per movie
df.groupby('movieId')['rating'].mean()

# Number of ratings per user
df.groupby('userId')['rating'].count()

# Multiple aggregations
df.groupby('movieId')['rating'].agg(['mean', 'count'])
```

---

## Mapping Values

```python
# Replace values using a dict
mapping = {1: 'low', 3: 'mid', 5: 'high'}
df['rating'].map(mapping)

# Apply a function to each value
df['rating'].apply(lambda x: round(x * 2) / 2)  # round to nearest 0.5
```

---

## Extracting NumPy Arrays

```python
df['rating'].values          # numpy array (np.float64)
df['rating'].to_numpy()      # same
df['rating'].tolist()        # plain Python list
```
