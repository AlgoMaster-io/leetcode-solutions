# [Leetcode 1146: Snapshot Array](https://leetcode.com/problems/snapshot-array/)

## Table of Contents
1. [Approach 1: Brute Force Approach](#approach-1)
2. [Approach 2: Using HashMap and Balanced Tree](#approach-2)
3. [Approach 3: Optimized Using Binary Search](#approach-3)

---

## Approach 1: Brute Force Approach

### Intuition
The Snapshot Array problem involves maintaining an array where each update is stored in "snapshots" to allow for retrieval later. The simplest approach is to maintain a list of arrays, taking a full copy of the current state each time a snapshot is taken. This method uses a straightforward brute force approach to store every version of the array.

### Approach

1. On initialization, create an empty list of snapshots.
2. Every time `set` is called, change the value at the specified index in the current array.
3. On `snap`, make a deep copy of the current array and save it in the list of snapshots.
4. On `get`, fetch the value from the specified snapshot from the saved list.

### Code

```cpp
class SnapshotArray {
public:
    vector<vector<int>> snapshots;
    vector<int> currentArray;
    
    SnapshotArray(int length) {
        currentArray.resize(length, 0);
    }
    
    void set(int index, int val) {
        currentArray[index] = val;
    }
    
    int snap() {
        snapshots.push_back(currentArray); // Save the current array as a new snapshot
        return snapshots.size() - 1; // Return the snapshot index
    }
    
    int get(int index, int snap_id) {
        return snapshots[snap_id][index];
    }
};
```

### Time Complexity
- `set`: O(1)
- `snap`: O(n) where n is the length of the array (due to copying the array)
- `get`: O(1)

### Space Complexity
- O(n * s) where s is the number of snapshots

---

## Approach 2: Using HashMap and Balanced Tree

### Intuition
By using a hashmap where each key corresponds to an index of the array, and the value is a map mapping snapshot IDs to values. This allows each index to track its values by snapshot ID efficiently.

### Approach

1. Use a map for each array index to store values against snapshot IDs.
2. On `set`, update the map with the current snapshot ID or update the value.
3. On `snap`, increment and return the snapshot ID counter.
4. On `get`, retrieve the value corresponding to the highest snapshot ID not exceeding the queried one.

### Code

```cpp
class SnapshotArray {
public:
    vector<map<int, int>> data;
    int snap_id;
    
    SnapshotArray(int length) : data(length), snap_id(0) {}

    void set(int index, int val) {
        data[index][snap_id] = val;
    }
    
    int snap() {
        return snap_id++;
    }
    
    int get(int index, int snap_id) {
        auto it = data[index].upper_bound(snap_id);
        if (it == data[index].begin()) return 0;
        return prev(it)->second;
    }
};
```

### Time Complexity
- `set`: O(log s) per element where s is the number of snapshots because of the map
- `snap`: O(1)
- `get`: O(log s)

### Space Complexity
- O(n * s) where each index can have its own set of snapshot values

---

## Approach 3: Optimized Using Binary Search

### Intuition
Instead of using a map, we can use a vector of pairs, where each pair contains a snapshot ID and value, and exploit binary search for faster lookups compared to the map structure.

### Approach

1. Use a vector of vectors of pairs.
2. During `set`, append the snapshot ID and value for the given index.
3. During `snap`, increment and return the snapshot counter.
4. During `get`, use binary search to find the appropriate value for a given snapshot ID.

### Code

```cpp
class SnapshotArray {
private:
    vector<vector<pair<int, int>>> data;
    int snap_id;

public:
    SnapshotArray(int length) : data(length), snap_id(0) {
        for (int i = 0; i < length; ++i) {
            // Initialize each index with a snapshot ID of -1 and value 0
            data[i].emplace_back(-1, 0);
        }
    }

    void set(int index, int val) {
        // If the last recorded snapshot ID is the current snap_id, replace it with the new value
        if (data[index].back().first == snap_id) {
            data[index].back().second = val;
        }
        else {
            data[index].emplace_back(snap_id, val);
        }
    }

    int snap() {
        return snap_id++;
    }

    int get(int index, int snap_id) {
        auto& vec = data[index];
        auto it = upper_bound(vec.begin(), vec.end(), make_pair(snap_id, INT_MAX));
        // Move back one step to get the largest snapshot ID ≤ snap_id
        it--;
        return it->second;
    }
};
```

### Time Complexity
- `set`: O(1) amortized
- `snap`: O(1)
- `get`: O(log s) due to using binary search on list of snapshots for each index

### Space Complexity
- O(n + u) where u is the number of times `set` is called

This approach is optimized for both time and space compared to the brute force method, particularly when the number of updates is small compared to the number of queries and snapshots.

