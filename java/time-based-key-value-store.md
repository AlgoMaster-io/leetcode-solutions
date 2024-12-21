# [Leetcode 981: Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approaches:

1. [Using HashMap with Linear Search (Basic)](#using-hashmap-with-linear-search-basic)
2. [Using HashMap with Binary Search (Optimal)](#using-hashmap-with-binary-search-optimal)

---

### Using HashMap with Linear Search (Basic)

#### Intuition:
The simplest approach to solve this problem is to use a `HashMap` where the keys are strings and the values are lists of pairs (timestamp, value). To perform the `get` operation, we can iterate over the list for a particular key and find the largest timestamp that is less than or equal to the given timestamp.

#### Steps:
1. We'll use a `HashMap<String, List<Pair<Integer, String>>>` to store keys with their associated timestamp-value pairs.
2. For the `set` operation, we'll add a pair of `(timestamp, value)` to the list corresponding to the key.
3. For the `get` operation, we'll iterate through the list associated with the given key and check for the largest timestamp that is less than or equal to the given timestamp.

#### Time Complexity:
- `set`: O(1) amortized time complexity.
- `get`: O(n) where n is the number of timestamp-value pairs for the key.

#### Space Complexity:
- O(n) to store all timestamp-value pairs.

```java
import java.util.*;

class TimeMap {
    private Map<String, List<Pair>> map;

    public TimeMap() {
        map = new HashMap<>();
    }

    public void set(String key, String value, int timestamp) {
        // Store the (timestamp, value) pair in the map
        map.computeIfAbsent(key, k -> new ArrayList<>()).add(new Pair(timestamp, value));
    }

    public String get(String key, int timestamp) {
        // If the key does not exist, return an empty string
        if (!map.containsKey(key)) return "";
        
        List<Pair> pairs = map.get(key);
        
        // Linear search to find the largest timestamp <= given timestamp
        String result = "";
        for (Pair pair : pairs) {
            if (pair.timestamp <= timestamp) {
                result = pair.value;
            } else {
                break;
            }
        }

        return result;
    }

    private static class Pair {
        int timestamp;
        String value;
        
        Pair(int t, String v) {
            timestamp = t;
            value = v;
        }
    }
}
```

---

### Using HashMap with Binary Search (Optimal)

#### Intuition:
An optimized approach improves the `get` operation by using binary search. Given that the timestamps are stored in sorted order by insertion, we can utilize binary search to quickly locate the largest timestamp that is less than or equal to the given timestamp.

#### Steps:
1. Similar to the previous approach, we'll use a `HashMap` with lists of pairs for each key.
2. For the `set` operation, the implementation remains the same.
3. For the `get` operation, use binary search on the list of pairs for the given key instead of linear search.

#### Time Complexity:
- `set`: O(1) amortized time complexity.
- `get`: O(log n), where n is the number of timestamp-value pairs for the key due to binary search.

#### Space Complexity:
- O(n) to store all timestamp-value pairs.

```java
import java.util.*;

class TimeMap {
    private Map<String, List<Pair>> map;

    public TimeMap() {
        map = new HashMap<>();
    }

    public void set(String key, String value, int timestamp) {
        // Store the (timestamp, value) pair in the map
        map.computeIfAbsent(key, k -> new ArrayList<>()).add(new Pair(timestamp, value));
    }

    public String get(String key, int timestamp) {
        // If the key does not exist, return an empty string
        if (!map.containsKey(key)) return "";
        
        List<Pair> pairs = map.get(key);
        
        // Binary search to find the largest timestamp <= given timestamp
        int left = 0, right = pairs.size() - 1;
        String result = "";
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (pairs.get(mid).timestamp <= timestamp) {
                result = pairs.get(mid).value; // Potential result
                left = mid + 1; // Look in the right half for a closer match
            } else {
                right = mid - 1; // Look in the left half
            }
        }

        return result;
    }

    private static class Pair {
        int timestamp;
        String value;
        
        Pair(int t, String v) {
            timestamp = t;
            value = v;
        }
    }
}
```

This optimal approach leverages binary search to significantly improve the efficiency of the `get` operation for the time-based key-value store, especially beneficial when the number of timestamp-value pairs grows large.

