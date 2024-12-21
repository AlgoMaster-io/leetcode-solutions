# [Leetcode 1146: Snapshot Array](https://leetcode.com/problems/snapshot-array/)

## Approaches
1. [Naive Approach](#naive-approach)
2. [Efficient Approach with Binary Search](#efficient-approach-with-binary-search)

### Naive Approach

#### Intuition:
The naive approach involves storing the entire array for each snapshot. Although this method is simple to implement, it is not efficient as it uses a lot of memory space if the array or the number of snapshots is large.

#### Implementation:
```java
import java.util.ArrayList;
import java.util.List;

class SnapshotArray {
    private List<int[]> history;
    private int[] array;

    // Initialize the SnapshotArray object with length n
    public SnapshotArray(int length) {
        array = new int[length];
        history = new ArrayList<>();
    }

    // Set the value at index to be val
    public void set(int index, int val) {
        array[index] = val;  // Directly set the value at the index
    }

    // Store the copy of current state of array in history and return snap id
    public int snap() {
        history.add(array.clone());  // Save the entire array
        return history.size() - 1;   // Return the snapshot id
    }

    // Return the value at index for the given snap id
    public int get(int index, int snap_id) {
        return history.get(snap_id)[index];  // Retrieve the value from the specific snapshot
    }
}
```

#### Time and Space Complexity
- `set`: O(1), because setting the value takes constant time.
- `snap`: O(n), where n is the length of the array, since we need to duplicate the array.
- `get`: O(1), as we directly fetch the value from a stored snapshot.
- Space: O(s * n), where s is the number of snaps and n is the length of the array.

### Efficient Approach with Binary Search

#### Intuition:
Instead of storing the entire array for each snapshot, we only store the changes made for each snapshot. This can be efficiently handled using a list of TreeMaps where the keys are indices of the array, and the values are another TreeMap that maps the snapshot id to the values that were set during that snapshot.

#### Implementation:
```java
import java.util.HashMap;
import java.util.TreeMap;

class SnapshotArray {
    private List<TreeMap<Integer, Integer>> arraySnapList;
    private int currentSnap;

    // Initialize the SnapshotArray object with length n
    public SnapshotArray(int length) {
        arraySnapList = new ArrayList<>();
        for (int i = 0; i < length; i++) {
            arraySnapList.add(new TreeMap<>());
            arraySnapList.get(i).put(0, 0);  // Initializing all indices with value 0 at snap_id = 0
        }
        currentSnap = 0;
    }

    // Set the value at index to be val
    public void set(int index, int val) {
        arraySnapList.get(index).put(currentSnap, val);
    }

    // Increment the snap id and return the previous id
    public int snap() {
        return currentSnap++;
    }

    // Return the value at index for the given snap id, using binary search to find the right value
    public int get(int index, int snap_id) {
        return arraySnapList.get(index).floorEntry(snap_id).getValue();
    }
}
```

#### Time and Space Complexity
- `set`: O(log S), where S is the number of snapshots taken, for the insertion in the TreeMap.
- `snap`: O(1), as we only increment a counter.
- `get`: O(log S), due to the binary search operation `floorEntry()` in TreeMap.
- Space: O(n + q), where n is the length of the array and q is the number of set operations until last snap.

