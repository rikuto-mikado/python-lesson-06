## What I Learned

## What Was Difficult

## Notes

### TF-IDF (Term Frequency - Inverse Document Frequency)

A technique to quantify how important a word is within a document relative to a collection of documents (corpus).

**TF-IDF = TF × IDF**

| Component | Full Name | What it measures |
|-----------|-----------|-----------------|
| TF | Term Frequency | How often a word appears in a single document |
| IDF | Inverse Document Frequency | How rare the word is across all documents |

A word scores high when it appears **frequently in one document** but **rarely across the corpus** — making it a meaningful signal for that document.

**Example:**

| Word | TF (in this movie) | IDF (rarity) | TF-IDF |
|------|--------------------|--------------|--------|
| `car` | high | high (only 1/6 movies) | **high → important** |
| `the` | high | low (appears in all movies) | **low → not important** |

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
