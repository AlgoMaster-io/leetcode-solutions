[Leetcode 1146: Snapshot Array](https://leetcode.com/problems/snapshot-array/)

## Approaches
1. [Naive Approach](#naive-approach)
2. [Efficient Approach using HashMap](#efficient-approach-using-hashmap)

### Naive Approach

**Intuition**:  
The straightforward approach involves maintaining a list for each index of the array which will store the values associated with each snapshot. Each time we call `snap()`, we store the current state of the array. On calling `get()`, we retrieve the appropriate value associated with the requested snapshot.

**Steps**:
1. Use a list of lists (or arrays) to simulate snapshot functionality.
2. Whenever `snap()` is called, add the current state of the array to a list.
3. Whenever `set()` is called, update the current array state.
4. For `get()`, simply retrieve the snapshot value by accessing the list of state arrays using the snapshot ID.

**Code**:
```csharp
public class SnapshotArray {
    private List<int[]> snapshots;
    private int currentSnapId;
    private int[] currentState;

    public SnapshotArray(int length) {
        snapshots = new List<int[]>();
        currentState = new int[length];
        currentSnapId = 0;
    }
    
    public void Set(int index, int val) {
        currentState[index] = val;
    }
    
    public int Snap() {
        snapshots.Add((int[])currentState.Clone()); // Store a copy of the current state
        return currentSnapId++;
    }
    
    public int Get(int index, int snapId) {
        return snapshots[snapId][index];
    }
}
```

**Time Complexity**:
- `Set`: O(1)
- `Snap`: O(n) (Because we are storing a cloned state of the array)
- `Get`: O(1)

**Space Complexity**: O(n * s) where `s` is the number of snapshots.

---

### Efficient Approach using HashMap

**Intuition**:  
To optimize space, instead of storing every element for each snapshot, store only the changes that occur. Use a dictionary for each index to track the value changes at different snapshot versions. This way, when `get()` is called, we can look back to the closest snapshot ID that includes a change for that index.

**Steps**:
1. Use a dictionary where each index maps to a list of pairs `(snapId, value)`.
2. For `set()`, simply add or update the current snapId with the value in the list of pairs for that index.
3. For `snap()`, increment the snapId and return the old snapId as the current snapshot ID.
4. For `get()`, perform a binary search (or linear search) on the list of pairs for that index to find the appropriate value for the given snapshot ID.

**Code**:
```csharp
using System;
using System.Collections.Generic;

public class SnapshotArray {
    private int currentSnapId;
    private Dictionary<int, List<(int snapId, int value)>> indexToSnapshots;

    public SnapshotArray(int length) {
        currentSnapId = 0;
        indexToSnapshots = new Dictionary<int, List<(int snapId, int value)>>();
        for (int i = 0; i < length; i++) {
            indexToSnapshots[i] = new List<(int snapId, int value)>() { (0, 0) }; // Initialize with snapId 0, default value 0
        }
    }

    public void Set(int index, int val) {
        var snapshots = indexToSnapshots[index];
        if (snapshots[snapshots.Count - 1].snapId == currentSnapId) {
            snapshots[snapshots.Count - 1] = (currentSnapId, val);
        } else {
            snapshots.Add((currentSnapId, val));
        }
    }

    public int Snap() {
        return currentSnapId++;
    }

    public int Get(int index, int snapId) {
        var snapshots = indexToSnapshots[index];
        int low = 0, high = snapshots.Count - 1;
        while (low < high) {
            int mid = (low + high + 1) / 2;
            if (snapshots[mid].snapId <= snapId) {
                low = mid;
            } else {
                high = mid - 1;
            }
        }
        return snapshots[low].value;
    }
}
```

**Time Complexity**:
- `Set`: O(1)
- `Snap`: O(1)
- `Get`: O(log m) where `m` is the number of changes for that index (using binary search)

**Space Complexity**: O(n + c) where `c` is the number of changes across all snapshots. 

The HashMap solution is significantly more space efficient, especially when there are fewer changes made compared to the number of snapshots taken.

