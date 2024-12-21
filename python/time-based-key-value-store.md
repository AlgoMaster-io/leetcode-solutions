# [Leetcode 981: Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approaches:
- [Approach 1: Brute-force with Linear Search](#approach-1-brute-force-with-linear-search)
- [Approach 2: Binary Search on Timestamp](#approach-2-binary-search-on-timestamp)

## Approach 1: Brute-force with Linear Search

### Intuition
The problem is to design a data structure to store a series of key-value pairs with associated timestamps and to retrieve the value for a key that has a timestamp less than or equal to a given timestamp. The simplest way to implement this is to use a dictionary where each key maps to a list of (value, timestamp) pairs. On querying, we perform a linear search to find the largest timestamp less than or equal to the query timestamp.

### Implementation

```python
class TimeMap:
    def __init__(self):
        """ Initialize the data structure. """
        self.store = {}

    def set(self, key: str, value: str, timestamp: int) -> None:
        """ Store the (value, timestamp) for the given key. """
        if key not in self.store:
            self.store[key] = []
        # Append the (value, timestamp) pair to the list for this key.
        self.store[key].append((value, timestamp))

    def get(self, key: str, timestamp: int) -> str:
        """ Retrieve the latest value for the given key with timestamp <= the provided timestamp. """
        if key not in self.store:
            return ""
        # List of (value, timestamp) pairs for the given key.
        values = self.store[key]
        # Linear search to find the right value for the key at the given timestamp.
        for value, ts in reversed(values):
            if ts <= timestamp:
                return value
        return ""
```

### Time and Space Complexity
- `set` operation: O(1) time, O(n) space for n `set` operations where each operation appends to the list.
- `get` operation: O(n) time in the worst case where we iterate over all timestamps for a specific key.
- Space Complexity: O(n) where n is the number of `set` operations due to storage of all key-value-timestamp tuples.

## Approach 2: Binary Search on Timestamp

### Intuition
To improve the efficiency of the `get` operation from linear to logarithmic time, we can exploit the fact that the timestamps for each key are monotonically increasing. We can store (value, timestamp) pairs and perform a binary search to quickly find the desired value for a timestamp.

### Implementation

```python
import bisect

class TimeMap:
    def __init__(self):
        """ Initialize the data structure. """
        self.store = {}

    def set(self, key: str, value: str, timestamp: int) -> None:
        """ Store the (value, timestamp) for the given key. """
        if key not in self.store:
            self.store[key] = []
        # Append the (value, timestamp) pair to the list for this key.
        self.store[key].append((timestamp, value))

    def get(self, key: str, timestamp: int) -> str:
        """ Retrieve the latest value for the given key with timestamp <= the provided timestamp. """
        if key not in self.store:
            return ""
        # Retrieve the list of (timestamp, value) pairs for the given key.
        lst = self.store[key]
        # Extract all timestamps for binary search.
        timestamps = [t[0] for t in lst]
        # Use binary search to find the rightmost timestamp less than or equal to the given timestamp.
        idx = bisect.bisect_right(timestamps, timestamp) - 1
        if idx >= 0:
            return lst[idx][1]
        return ""
```

### Time and Space Complexity
- `set` operation: O(1) time for appending. Space grows linearly with the number of set operations.
- `get` operation: O(log n) time due to binary search through timestamps.
- Space Complexity: O(n) similar to the brute-force because we are storing all (timestamp, value) pairs. 

The binary search significantly optimizes the `get` operation, making it efficient even with a large number of timestamps.

