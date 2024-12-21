# [Leetcode 981: Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approaches
1. [Naive Approach](#naive-approach)
2. [Optimal Approach using Binary Search](#optimal-approach-using-binary-search)

---

## Naive Approach

### Intuition
The naive solution involves storing all the values associated with a key along with their timestamps. Each time a `get` request is made, we traverse through all stored values for that key to find the one with the largest timestamp that does not exceed the specified timestamp.

### Code
```cpp
class TimeMap {
public:
    // A map from each key to a vector of pairs containing a timestamp and its associated value
    unordered_map<string, vector<pair<int, string>>> store;
    
    // Store the key-value pair with the associated timestamp
    void set(string key, string value, int timestamp) {
        // Append the value and its timestamp to the list of pairs for that key
        store[key].emplace_back(timestamp, value);
    }
    
    // Retrieve the value with the largest timestamp that does not exceed the given timestamp
    string get(string key, int timestamp) {
        // If the key does not exist, return an empty string
        if (store.find(key) == store.end()) {
            return "";
        }
        
        // Iterate through the list of (timestamp, value) pairs for that key
        for (auto it = store[key].rbegin(); it != store[key].rend(); ++it) {
            // If a pair's timestamp is less than or equal to the given timestamp, return its value
            if (it->first <= timestamp) {
                return it->second;
            }
        }
        
        // No suitable value was found for the given timestamp
        return "";
    }
};
```

### Complexity Analysis
- **Time Complexity**: 
   - `set`: O(1) - Insertion in a map is on average O(1).
   - `get`: O(n) - Searching the vector of pairs is O(n) where n is the number of stored values for that key.
- **Space Complexity**: O(n) - Storing each key-value pair takes O(1) per insertion, where `n` is the total number of `set` operations.

---

## Optimal Approach using Binary Search

### Intuition
To improve upon the naive approach, we can leverage the sorted property of timestamps. Instead of iterating through all pairs, we can use binary search to quickly find the right value for a `get` operation. This requires us to keep the values sorted by timestamp.

### Code
```cpp
class TimeMap {
public:
    // Store key to a sorted list of (timestamp, value) pairs, allowing efficient search
    unordered_map<string, vector<pair<int, string>>> store;

    void set(string key, string value, int timestamp) {
        // Append the timestamp and associated value
        store[key].emplace_back(timestamp, value);
    }
    
    // Use binary search to efficiently find the right value
    string get(string key, int timestamp) {
        // If the key does not exist, return an empty string
        if (store.find(key) == store.end()) {
            return "";
        }
        
        const auto& values = store[key];
        
        // We use the function lower_bound to find the first element that is strictly greater than the timestamp
        auto it = upper_bound(values.begin(), values.end(), make_pair(timestamp, string("")), 
                              [](const pair<int, string>& a, const pair<int, string>& b) {
                                  return a.first < b.first;
                              });
        
        // If the first element is selected by upper_bound, return the empty string
        if (it == values.begin()) {
            return "";
        }

        // Move back to get the last valid timestamp
        --it;
        
        return it->second;
    }
};
```

### Complexity Analysis
- **Time Complexity**: 
   - `set`: O(1) - Appending a value to a vector is amortized O(1).
   - `get`: O(log n) - Binary search on the sorted vector.
- **Space Complexity**: O(n) - Similar to the naive approach, storing each key-timestamp-value triple.

