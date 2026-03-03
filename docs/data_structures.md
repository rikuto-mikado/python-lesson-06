# Data Structures

## Choosing the Right Structure

| Structure | Ordered | Duplicates | Mutable | Use when |
|---|---|---|---|---|
| `list` | Yes | Yes | Yes | Sequence of items, order matters |
| `set` | No | No | Yes | Uniqueness check, membership test |
| `dict` | Yes (3.7+) | Keys: No | Yes | Key-value mapping |
| `tuple` | Yes | Yes | No | Fixed record, return multiple values |

---

## list

```python
items = []
items.append(x)       # add to end
items.insert(0, x)    # add at index
items.remove(x)       # remove first occurrence of x
items.pop()           # remove and return last item
items.pop(0)          # remove and return item at index 0
len(items)            # number of elements
x in items            # membership check (slow for large lists)
```

---

## set

```python
seen = set()
seen.add(x)           # add element (no duplicates)
seen.remove(x)        # remove (raises error if not found)
seen.discard(x)       # remove (no error if not found)
x in seen             # membership check (fast, O(1))
seen_a & seen_b       # intersection
seen_a | seen_b       # union
seen_a - seen_b       # difference
```

Common real-world pattern — collect unique IDs:

```python
seen_users = set()
for _, row in df.iterrows():
    seen_users.add(row['userId'])
# faster and simpler than checking a list
```

---

## dict

```python
mapping = {}
mapping[key] = value          # set value
mapping.get(key, default)     # get with fallback
key in mapping                # check if key exists
mapping.items()               # iterate as (key, value) pairs
mapping.keys()                # all keys
mapping.values()              # all values
```

Common real-world pattern — assign sequential inner IDs:

```python
id_map = {}
for raw_id in raw_ids:
    if raw_id not in id_map:
        id_map[raw_id] = len(id_map)  # assign next available int
```

---

## tuple

```python
record = (user_id, movie_id, rating)  # fixed, immutable
u, i, r = record                      # unpacking
```

Use tuples (not lists) for records that should not change — e.g., a single rating entry.
Tuples are also sortable: `sorted(list_of_tuples)` sorts lexicographically by first element, then second, etc.
