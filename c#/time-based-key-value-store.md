# [Leetcode 981: Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Hash Table with Linear Search on List](#approach-2-hash-table-with-linear-search-on-list)
- [Approach 3: Hash Table with Binary Search on Sorted List](#approach-3-hash-table-with-binary-search-on-sorted-list)

---

## Approach 1: Brute Force

### Intuition
The problem can be tackled by storing all key-timestamp-value tuples and simply iterating through them to find the value associated with the largest timestamp that is less than or equal to the given timestamp. This brute force approach does not optimize for lookup time, leading to inefficiencies.

### Strategy
1. Store every put operation in a list as a tuple of (key, timestamp, value).
2. During a get operation, iterate through the list and check for all entries with: 
   - the same key as the get request 
   - timestamp less than or equal to the requested timestamp.
3. Return the value with the largest feasible timestamp.

### Code

```csharp
public class TimeMap {
    private List<(string key, int timestamp, string value)> store;

    public TimeMap() {
        store = new List<(string key, int timestamp, string value)>();
    }
    
    public void Set(string key, string value, int timestamp) {
        store.Add((key, timestamp, value));
    }
    
    public string Get(string key, int timestamp) {
        string result = "";
        int maxTimestamp = -1;
        
        // Traverse the list to find the suitable entry
        foreach (var entry in store) {
            if (entry.key == key && entry.timestamp <= timestamp && entry.timestamp > maxTimestamp) {
                // Update maxTimestamp if this entry has a larger timestamp
                maxTimestamp = entry.timestamp;
                result = entry.value;
            }
        }
        return result;
    }
}
```

### Complexity
- Time Complexity: 
  - Set: O(1) for adding to the list.
  - Get: O(N) where N is the number of entries due to the full linear scan.
- Space Complexity: O(N) for storing all entries.

---

## Approach 2: Hash Table with Linear Search on List

### Intuition
Using a dictionary (hash table) where the key maps to a list of (timestamp, value) pairs optimizes storage and organization. Despite improvement, this solution still employs linear search through timestamps.

### Strategy
1. Use a HashTable to map keys to a list of (timestamp, value) pairs.
2. For put operations, simply append the pair to the list corresponding to the key.
3. For get operations, iterate through the list associated with the key, searching for the largest valid timestamp.

### Code

```csharp
public class TimeMap {
    private Dictionary<string, List<(int timestamp, string value)>> store;

    public TimeMap() {
        store = new Dictionary<string, List<(int timestamp, string value)>>();
    }
    
    public void Set(string key, string value, int timestamp) {
        if (!store.ContainsKey(key)) {
            store[key] = new List<(int timestamp, string value)>();
        }
        store[key].Add((timestamp, value));
    }
    
    public string Get(string key, int timestamp) {
        if (!store.ContainsKey(key)) {
            return "";
        }
        
        string result = "";
        int maxTimestamp = -1;
        
        foreach (var entry in store[key]) {
            if (entry.timestamp <= timestamp && entry.timestamp > maxTimestamp) {
                maxTimestamp = entry.timestamp;
                result = entry.value;
            }
        }
        return result;
    }
}
```

### Complexity
- Time Complexity: 
  - Set: O(1) for appending to the list in the hash table.
  - Get: O(M) per get operation where M is the number of timestamps for a key (worst-case linear search).
- Space Complexity: O(N) where N is the total number of timestamp-value pairs stored.

---

## Approach 3: Hash Table with Binary Search on Sorted List

### Intuition
For more efficient retrieval of timestamps, the list of timestamps should be sorted. We can exploit binary search to swiftly locate the largest timestamp not exceeding the request.

### Strategy
1. Use a HashTable to map keys to a list of (timestamp, value) pairs, which are maintained in sorted order by timestamp.
2. For put operations, append, assuming timestamps will naturally be ordered by usage.
3. For get operations, conduct a binary search to efficiently pinpoint the relevant timestamp.

### Code

```csharp
public class TimeMap {
    private Dictionary<string, List<(int timestamp, string value)>> store;

    public TimeMap() {
        store = new Dictionary<string, List<(int timestamp, string value)>>();
    }
    
    public void Set(string key, string value, int timestamp) {
        if (!store.ContainsKey(key)) {
            store[key] = new List<(int timestamp, string value)>();
        }
        store[key].Add((timestamp, value));
    }
    
    public string Get(string key, int timestamp) {
        if (!store.ContainsKey(key) || store[key].Count == 0) {
            return "";
        }
        
        var entries = store[key];
        int left = 0;
        int right = entries.Count - 1;
        string result = "";

        // Binary search for the largest timestamp <= the given timestamp
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (entries[mid].timestamp <= timestamp) {
                result = entries[mid].value;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return result;
    }
}
```

### Complexity
- Time Complexity: 
  - Set: O(1) assuming timestamps are naturally appended in increasing order.
  - Get: O(log M) with binary search over the list of timestamps.
- Space Complexity: O(N) for storing all timestamp-value pairs, similar to previous approaches.

These approaches incrementally optimize the process of getting the desired value associated with the timestamp, ultimately providing efficient lookups using binary search in the hash table lists.

