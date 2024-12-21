# [Leetcode 1146: Snapshot Array](https://leetcode.com/problems/snapshot-array/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Efficient Approach using HashMap and Binary Search](#efficient-approach-using-hashmap-and-binary-search)

### Brute Force Approach

#### Intuition
The brute force approach involves maintaining a dictionary for each snapshot taken. When we take a snapshot, we store the entire array at that particular moment. Whenever we need to retrieve a value for a certain index from a snapshot, we just return the stored value from that dictionary.

This approach is straightforward, but it is inefficient, especially in terms of space complexity, because we store the entire array for each snapshot, which can lead to high memory usage.

#### Steps:
1. Initialize an array of dictionaries.
2. For each snapshot, store the current state of the array in a new dictionary and append it to the array.
3. For each `set` operation, update the current state of the array.
4. For each `get` operation, retrieve the value from the stored snapshot state.

#### Time Complexity:
- `O(1)` for `set()`. 
- `O(1)` for `get()`.
- `O(n)` for `snap()` where `n` is the length of the array.

#### Space Complexity:
- `O(n * s)`, where `n` is the length of the array and `s` is the number of snapshots taken.

```javascript
class SnapshotArray {
    constructor(length) {
        this.snapshots = [];
        this.currentArray = Array(length).fill(0);
    }

    set(index, val) {
        // Update the current state of the array with new value at 'index'
        this.currentArray[index] = val;
    }

    snap() {
        // Store a snapshot (copy of the current array) in snapshots list
        this.snapshots.push([...this.currentArray]);
        // Return the index of this snapshot
        return this.snapshots.length - 1;
    }

    get(index, snap_id) {
        // Retrieve the value from the stored snapshot at 'snap_id'
        return this.snapshots[snap_id][index];
    }
}
```

### Efficient Approach using HashMap and Binary Search

#### Intuition
To optimize space usage, we maintain a history of changes at each index. We use a hashmap where the key is the index and the value is a list of tuples. Each tuple contains a snapshot id and the value at that snapshot. 

When retrieving the value, we perform a binary search to find the highest snapshot id less than or equal to the requested `snap_id` for a given index. This reduces memory usage by only storing changes rather than the entire array and allows for quick retrieval using binary search.

#### Steps:
1. Initialize a hashmap to store changes. Use a current snapshot count initialized to -1.
2. For each `set` operation, record the value in the hashmap with the current snapshot id.
3. For each `snap` operation, increment the snapshot count and return it.
4. For each `get` operation, perform a binary search in the hashmap to find the appropriate value.

#### Time Complexity:
- `O(1)` for `set()`.
- `O(log n)` for `get()` using binary search, where `n` is the number of changes at a particular index.
- `O(1)` for `snap()`.

#### Space Complexity:
- `O(n + c)`, where `n` is the number of indexes and `c` is the number of changes across all snapshots.

```javascript
class SnapshotArray {
    constructor(length) {
        this.snap_id = -1;
        this.changes = Array.from({ length }, () => []);
    }

    set(index, val) {
        // Record the value along with the current snapshot id
        if (this.changes[index].length === 0 || this.changes[index][this.changes[index].length - 1][0] !== this.snap_id) {
            this.changes[index].push([this.snap_id, val]);
        } else {
            this.changes[index][this.changes[index].length - 1][1] = val;
        }
    }

    snap() {
        // Increment the snapshot id and return it
        this.snap_id++;
        return this.snap_id;
    }

    get(index, snap_id) {
        // Binary search for the largest snap_id less than or equal to the requested snap_id
        let changes = this.changes[index];
        let left = 0;
        let right = changes.length - 1;
        while (left <= right) {
            const mid = Math.floor((left + right) / 2);
            if (changes[mid][0] <= snap_id) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        // If no changes at this index before the snap_id, return 0 as initial value
        return right >= 0 ? changes[right][1] : 0;
    }
}
```

This solution efficiently handles large numbers of snapshots and updates while employing binary search to quickly access previous values, optimizing both time and space.

