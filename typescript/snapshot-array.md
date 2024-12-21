# [Leetcode 1146: Snapshot Array](https://leetcode.com/problems/snapshot-array/)

## Table of Contents
1. [Approach 1: Brute Force Approach](#approach-1)
2. [Approach 2: Using HashMap with History](#approach-2)
3. [Approach 3: Optimized Using Map with Snapshots](#approach-3)

### Approach 1: Brute Force Approach

In this basic approach, we use a 2-dimensional array to keep track of the value of each index after every operation. This is straightforward but inefficient in terms of both time and space, especially with a large number of operations.

#### Plan
- Maintain a 2D array where each row will represent the snapshot at that moment.
- Each cell in the row reflects the value at that specific index.
- On `snap`, we'll create a copy.
- For `get`, return the value from the respective snapshot.

```typescript
class SnapshotArray {
    private snaps: number[][];
    private currentArray: number[];
    private snapId: number;

    constructor(length: number) {
        this.snaps = [];
        this.currentArray = new Array(length).fill(0);
        this.snapId = 0;
    }

    // Sets the element at index to be equal to val
    set(index: number, val: number): void {
        this.currentArray[index] = val;
    }

    // Takes a snapshot of the array, and returns the snapshot ID
    snap(): number {
        this.snaps[this.snapId] = [...this.currentArray];
        return this.snapId++;
    }

    // Returns the value at index at the time we took the snapshot with the given snapId
    get(index: number, snap_id: number): number {
        return this.snaps[snap_id][index];
    }
}
```
#### Complexity Analysis
- **Time Complexity**: 
  - `set()`: O(1)
  - `snap()`: O(n), where `n` is the length of the array (due to copying the array).
  - `get()`: O(1)
- **Space Complexity**: O(s * n), where `s` is the number of snapshots and `n` is the length of the array.

### Approach 2: Using HashMap with History

Instead of maintaining the entire array for each snapshot, we can optimize by maintaining a history of changes for each index.

#### Plan
- Maintain an array of maps, where each map represents the changes of an index.
- Store only changes, and each map entry will contain a snap_id and its value.
- On `snap`, increment the snap_id, storing the changes.
- `get` will traverse the changes of an index to find the latest applicable snap_id value.

```typescript
class SnapshotArray {
    private snaps: Map<number, number>[];
    private currentSnap: number;

    constructor(length: number) {
        this.snaps = Array.from({ length }, () => new Map<number, number>());
        this.currentSnap = 0;
    }

    set(index: number, val: number): void {
        this.snaps[index].set(this.currentSnap, val);
    }

    snap(): number {
        return this.currentSnap++;
    }

    get(index: number, snap_id: number): number {
        const changes = this.snaps[index];
        while (snap_id >= 0) {
            if (changes.has(snap_id)) {
                return changes.get(snap_id) as number;
            }
            snap_id--;
        }
        return 0; // Return default value if no earlier changes were found
    }
}
```
#### Complexity Analysis
- **Time Complexity**: 
  - `set()`: O(1)
  - `snap()`: O(1)
  - `get()`: O(log s), where `s` is the number of snapshots.
- **Space Complexity**: O(n + c), where `n` is the length of the array and `c` is the number of changes.

### Approach 3: Optimized Using Map with Snapshots

The optimized way is to enhance the storage by storing only necessary snapshots efficiently. This approach is apt for handling even a large number of `set` and `get` operations efficiently.

#### Plan
- Utilize a map storing only the latest change for each index per snap_id.
- Efficiently retrieve the last known value using binary search techniques on snapshots.

```typescript
class SnapshotArray {
    private snaps: Map<number, number>[];
    private currentSnap: number;

    constructor(length: number) {
        this.snaps = Array.from({ length }, () => new Map<number, number>([[0, 0]]));
        this.currentSnap = 0;
    }

    set(index: number, val: number): void {
        this.snaps[index].set(this.currentSnap, val);
    }

    snap(): number {
        return this.currentSnap++;
    }

    get(index: number, snap_id: number): number {
        const entries = Array.from(this.snaps[index].entries());
        let left = 0;
        let right = entries.length - 1;
        
        while (left < right) {
            let mid = Math.floor((left + right + 1) / 2);
            if (entries[mid][0] <= snap_id) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        
        return entries[left][1];
    }
}
```
#### Complexity Analysis
- **Time Complexity**: 
  - `set()`: O(1)
  - `snap()`: O(1)
  - `get()`: O(log c), where `c` is the number of changes recorded at that index due to binary search.
- **Space Complexity**: O(n + c), where `n` is the length of the array and `c` is the number of changes.

