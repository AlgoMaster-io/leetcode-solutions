# [1146. Snapshot Array](https://leetcode.com/problems/snapshot-array/)

## Approaches

- [Approach 1: Brute Force Using Multidimensional Array](#approach-1)
- [Approach 2: Using a HashMap with Snapshot IDs](#approach-2)
- [Approach 3: Using TreeMap (SortedMap) for Efficient Snap Handling](#approach-3)

## Approach 1: Brute Force Using Multidimensional Array

### Intuition
The simplest way to handle snapshots is to simulate them using a multidimensional array where each new snapshot is a new row in the array. This way, each snapshot can easily be accessed by its ID—the row index.

### Implementation
```go
type SnapshotArray struct {
    snaps [][]int
    current []int
}

func Constructor(length int) SnapshotArray {
    return SnapshotArray{
        snaps: make([][]int, 0),
        current: make([]int, length),
    }
}

func (this *SnapshotArray) Set(index int, val int)  {
    this.current[index] = val // Set the value in the current array state
}

func (this *SnapshotArray) Snap() int {
    this.snaps = append(this.snaps, append([]int(nil), this.current...)) // Save a snapshot of the current state
    return len(this.snaps) - 1 // Return the ID of the snapshot
}

func (this *SnapshotArray) Get(index int, snap_id int) int {
    return this.snaps[snap_id][index] // Retrieve the value from the snapshot
}
```

### Complexity
- **Time Complexity:** `O(n)` for `Set` and constant `O(1)` for `Get`, `O(n)` for `Snap`
- **Space Complexity:** `O(n * m)` where `n` is the number of snaps and `m` is the length of the array

## Approach 2: Using a HashMap with Snapshot IDs

### Intuition
Instead of storing each entire array snapshot, we can store only the changes. We can optimize space by storing an array of maps which store the index-value pairs that have been modified since the last snapshot.

### Implementation
```go
type SnapshotArray struct {
    data []map[int]int
    snapID int
}

func Constructor(length int) SnapshotArray {
    return SnapshotArray{
        data: make([]map[int]int, 1),
        snapID: 0,
    }
}

func (this *SnapshotArray) Set(index int, val int)  {
    if this.data[this.snapID] == nil {
        this.data[this.snapID] = make(map[int]int)
    }
    this.data[this.snapID][index] = val // Set the value in the current snapshot mapping
}

func (this *SnapshotArray) Snap() int {
    this.data = append(this.data, make(map[int]int)) // Start a new map for the next set of changes
    this.snapID++
    return this.snapID - 1
}

func (this *SnapshotArray) Get(index int, snap_id int) int {
    for i := snap_id; i >= 0; i-- { // Walk back in snapshots
        if val, exists := this.data[i][index]; exists {
            return val
        }
    }
    return 0 // Default value if no modification has been made
}
```

### Complexity
- **Time Complexity:** `O(n)` for worst-case `Get`; `O(1)` for `Set`, `O(1)` for `Snap`
- **Space Complexity:** `O(m)` average, with `m` as the number of distinct modifications

## Approach 3: Using TreeMap (SortedMap) for Efficient Snap Handling

### Intuition
This approach optimizes even further by using a sorted dictionary for each index. This allows quick access to the most recent value before a given snapshot.

### Implementation
```go
import "math"

type SnapshotArray struct {
    snaps []map[int]int
    curSnap map[int]int
    snapID int
}

func Constructor(length int) SnapshotArray {
    return SnapshotArray{
        snaps: make([]map[int]int, 0),
        curSnap: make(map[int]int),
        snapID: 0,
    }
}

func (this *SnapshotArray) Set(index int, val int)  {
    this.curSnap[index] = val // Directly map the latest set value for the index
}

func (this *SnapshotArray) Snap() int {
    // Store the snapshot of current changes
    snapCopy := make(map[int]int)
    for key, value := range this.curSnap {
        snapCopy[key] = value
    }
    this.snaps = append(this.snaps, snapCopy)
    this.curSnap = make(map[int]int) // Reset current modifications map
    this.snapID++
    return this.snapID - 1
}

func (this *SnapshotArray) Get(index int, snap_id int) int {
    for i := snap_id; i >= 0; i-- {
        if value, exists := this.snaps[i][index]; exists {
            return value
        }
    }
    return 0 // Default return value if never set
}
```

### Complexity
- **Time Complexity:** Each `Get`: average `O(log n)`, `Set`: `O(1)`, `Snap`: O(n) space copying
- **Space Complexity:** `O(n)` for each differently-set index per snapshot

By using techniques like only storing changed index values or leveraging sorted maps, we can greatly optimize the space and computation required for handling large arrays through many snapshots.

