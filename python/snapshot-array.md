# [Leetcode 1146: Snapshot Array](https://leetcode.com/problems/snapshot-array/)

## Approach

1. [Basic Approach: Using Nested Dictionary](#basic-approach)
2. [Optimized Approach: Efficient Snapshot and Binary Search](#optimized-approach)

---

### Basic Approach: Using Nested Dictionary

In this approach, we maintain a dictionary where each element is a dictionary mapping the snapshot id to the element's value. This ensures that each snapshot stores each element's value at that particular time.

**Intuition:**

- Create a list of dictionaries where each index points to a dictionary mapping snapshot id to the value at that index during that snapshot.
- Initially, all dictionaries are empty, representing no value has been set.
- On `set(index, val)`, update the dictionary at the index with the current snapshot id and the value.
- On `snap()`, increment the snapshot id counter and return the old snapshot id.
- On `get(index, snapshot_id)`, return the value corresponding to the snapshot id from the dictionary at the specified index.

**Code:**

```python
class SnapshotArray:
    def __init__(self, length: int):
        self.array = [{} for _ in range(length)]  # list of dictionaries
        self.snap_id = 0

    def set(self, index: int, val: int) -> None:
        # Set value at current snapshot
        self.array[index][self.snap_id] = val

    def snap(self) -> int:
        # Increment snap_id and return the previous one
        self.snap_id += 1
        return self.snap_id - 1

    def get(self, index: int, snap_id: int) -> int:
        # Find largest snapshot id less than or equal to the requested snap_id
        while snap_id >= 0 and snap_id not in self.array[index]:
            snap_id -= 1
        return self.array[index][snap_id] if snap_id >= 0 else 0
```

**Time Complexity:**

- `set`: O(1)
- `snap`: O(1)
- `get`: O(L), where L is the number of snapshots

**Space Complexity:** O(N), where N is the length of the array (storing dictionaries for each index)

---

### Optimized Approach: Efficient Snapshot and Binary Search

This approach improves the `get` operation using binary search. Instead of storing a dictionary for each index, store a list of tuples containing (snapshot_id, value) for each index. This allows efficient search of the nearest snapshot_id with binary search.

**Intuition:**

- Use a list of lists where each sublist stores tuples of (snapshot_id, value).
- On `set(index, val)`, append or update the last element of the list of tuples at the index if the last snapshot is the current snapshot.
- On `snap()`, increment and return the snapshot id.
- On `get(index, snapshot_id)`, use binary search to find the closest snapshot id less than or equal to the requested snapshot id.

**Code:**

```python
import bisect

class SnapshotArray:
    def __init__(self, length: int):
        self.array = [[(0, 0)] for _ in range(length)]  # list of tuples
        self.snap_id = 0

    def set(self, index: int, val: int) -> None:
        # Appends or updates the value at current snapshot
        if self.array[index][-1][0] == self.snap_id:
            # update if the last snap_id is the current snap
            self.array[index][-1] = (self.snap_id, val)
        else:
            self.array[index].append((self.snap_id, val))

    def snap(self) -> int:
        # Increment snap_id and return the previous one
        self.snap_id += 1
        return self.snap_id - 1

    def get(self, index: int, snap_id: int) -> int:
        # Binary search for the closest snapshot id <= requested snap_id
        pos = bisect.bisect(self.array[index], (snap_id + 1, float('-inf'))) - 1
        return self.array[index][pos][1]
```

**Time Complexity:**

- `set`: O(1)
- `snap`: O(1)
- `get`: O(log S), where S is the number of set operations performed on an index

**Space Complexity:** O(N + S), where N is the length of the array and S is the number of set operations (for storing tuples for each index)

