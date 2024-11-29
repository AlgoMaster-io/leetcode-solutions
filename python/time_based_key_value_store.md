# 981. [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approach: HashMap with Binary Search

### Solution
python
```python
# Time Complexity:
#   - set: O(1)
#   - get: O(log(n)), where n is the number of timestamps for a given key
# Space Complexity: O(n), where n is the total number of key-timestamp-value entries
from collections import defaultdict
from bisect import bisect_right

class TimeMap:
    def __init__(self):
        # Initialize a dictionary to store the key and list of (timestamp, value) pairs
        self.map = defaultdict(list)
    
    def set(self, key: str, value: str, timestamp: int) -> None:
        # Append the pair (timestamp, value) to the list for the given key
        self.map[key].append((timestamp, value))

    def get(self, key: str, timestamp: int) -> str:
        if key not in self.map:
            return ""

        pairs = self.map[key]
        # Use binary search to find the rightmost timestamp less than or equal to the given timestamp
        i = bisect_right(pairs, (timestamp, chr(255))) - 1

        # Return "" if no such timestamp is found
        return pairs[i][1] if i >= 0 else ""


# Example of usage:
# obj = TimeMap()
# obj.set("foo", "bar", 1)
# param_2 = obj.get("foo", 1)
```


