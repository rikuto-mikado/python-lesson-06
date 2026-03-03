## What I Learned

- **Content-Based Filtering (CBF)**: A recommendation system that suggests items similar to what users liked by comparing item features (text overviews in this case)
- **Text Vectorization**: Converting movie overviews into numerical vectors using TF-IDF scores
- **Similarity Computation**: Using `linear_kernel()` to calculate cosine similarity between movie vectors to find recommendations
- **NumPy Array Manipulation**: Extracting and sorting similarity scores using `enumerate()` and `lambda` functions
- **Pandas DataFrame Operations**: Filtering by conditions, extracting values using `.iloc[]`, and data type conversions

## What Was Difficult

- **Spelling errors**: `sklearn.metrics.parwise` should be `sklearn.metrics.pairwise`
- **NumPy type display**: `np.float64()` wrapper in output — fixed by converting to native Python list with `.tolist()`
- **Bracket matching**: Missing closing parentheses in function definitions
- **Pandas column indexing**: Correct syntax is `movies["title"].iloc[indices]`, not `movies["title".iloc[indices]`
- **Lambda functions in sorted()**: Understanding how `key=lambda x: x[1]` extracts the second element from tuples for sorting criteria

## Reference Docs

| Doc | Topics |
|---|---|
| [Python Conventions](docs/python_conventions.md) | `_` for unused variables, NumPy type casting, `*` unpacking |
| [Data Structures](docs/data_structures.md) | list / set / dict / tuple — methods and when to use each |
| [Sparse Matrix](docs/sparse_matrix.md) | CSR/COO formats, building, iterating, memory comparison |
| [Pandas Patterns](docs/pandas_patterns.md) | iterrows, factorize, filtering, groupby, mapping |

---

## Notes

### TF-IDF (Term Frequency - Inverse Document Frequency)

A technique to quantify how important a word is within a document relative to a collection of documents (corpus).

**TF-IDF = TF × IDF**

| Component | Full Name                  | What it measures                              |
| --------- | -------------------------- | --------------------------------------------- |
| TF        | Term Frequency             | How often a word appears in a single document |
| IDF       | Inverse Document Frequency | How rare the word is across all documents     |

A word scores high when it appears **frequently in one document** but **rarely across the corpus** — making it a meaningful signal for that document.

**Example:**

| Word  | TF (in this movie) | IDF (rarity)                | TF-IDF                  |
| ----- | ------------------ | --------------------------- | ----------------------- |
| `car` | high               | high (only 1/6 movies)      | **high → important**    |
| `the` | high               | low (appears in all movies) | **low → not important** |

Common words like "the" or "a" are removed via `stop_words="english"` before scoring.

**Usage in Content-Based Filtering:**

```python
from sklearn.feature_extraction.text import TfidfVectorizer

tfidf = TfidfVectorizer(stop_words="english")
tfidf_matrix = tfidf.fit_transform(movies["overview"])  # shape: (n_movies, n_words)

# View as DataFrame
pandas.DataFrame(tfidf_matrix.toarray(), columns=tfidf.get_feature_names_out())
```

Each movie's overview is converted into a vector of TF-IDF scores, one per word. These vectors are then used to compute similarity between movies.
