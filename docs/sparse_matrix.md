# Sparse Matrix (scipy)

## What is a Sparse Matrix?

A matrix where most values are 0. Instead of storing all zeros, sparse formats only store non-zero entries — saving memory dramatically.

Example: a user-item rating matrix with 671 users and 163,949 movies has ~110M cells, but only 100,004 are rated (0.09% density).

---

## Formats

| Format | Full name | Best for |
|---|---|---|
| `csr_matrix` | Compressed Sparse Row | Row slicing, matrix-vector multiply |
| `csc_matrix` | Compressed Sparse Column | Column slicing |
| `coo_matrix` | Coordinate | Building / converting matrices |

---

## Building a CSR Matrix

```python
from scipy.sparse import csr_matrix

# csr_matrix((data, (row_indices, col_indices)))
user_item_matrix = csr_matrix(
    (ratings['rating'].values,        # non-zero values
     (ratings['userId'].values - 1,   # row indices (0-based)
      ratings['movieId'].values - 1)) # col indices (0-based)
)
```

Note: IDs are 1-based in the CSV, so subtract 1 to make them 0-based indices.

---

## Useful Attributes

```python
matrix.shape      # (n_rows, n_cols)
matrix.nnz        # number of non-zero entries
matrix.dtype      # data type (e.g. float64)
```

---

## Converting Formats

```python
coo = matrix.tocoo()   # → COO format
csr = matrix.tocsr()   # → CSR format
arr = matrix.toarray() # → dense numpy array (careful: huge memory for large matrices)
```

---

## Iterating Over Non-Zero Entries

COO format exposes three parallel arrays:

```python
coo = matrix.tocoo()
coo.row    # array of row indices
coo.col    # array of col indices
coo.data   # array of values

# Combine into list of (user, movie, rating) tuples
all_ratings = list(zip(coo.row, coo.col, coo.data))

# Cast to plain Python types and sort
sorted((int(u), int(i), float(r)) for u, i, r in all_ratings)
```

---

## Memory Comparison

```python
import numpy as np

dense  = np.zeros((671, 163949))           # ~850 MB
sparse = csr_matrix((671, 163949))         # ~0 MB (only stores non-zeros)
```
