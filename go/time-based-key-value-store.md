## [Leetcode 981: Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

### Approaches:

1. [Basic Approach: Brute Force with Linear Search](#basic-approach)
2. [Intermediate Approach: Binary Search](#intermediate-approach)
3. [Optimal Approach: HashMap with Sorted List](#optimal-approach)

---

### Basic Approach: Brute Force with Linear Search

#### Intuition:
The problem requires storing values associated with keys and timestamps, and later retrieving the value for a key at the largest timestamp less or equal to the given timestamp. The basic idea is to store all the values linearly and search through them when needed.

#### Approach:
- Use a map to store each key with a list of pairs (value, timestamp).
- Each time we want to retrieve a value for a timestamp, linearly search through the list of pairs to find the applicable one.

#### Code:
```go
type Pair struct {
    value     string
    timestamp int
}

type TimeMap struct {
    store map[string][]Pair
}

func Constructor() TimeMap {
    return TimeMap{store: make(map[string][]Pair)}
}

func (this *TimeMap) Set(key string, value string, timestamp int) {
    this.store[key] = append(this.store[key], Pair{value, timestamp})
}

func (this *TimeMap) Get(key string, timestamp int) string {
    pairs, found := this.store[key]
    if !found {
        return ""
    }
    for i := len(pairs) - 1; i >= 0; i-- {
        if pairs[i].timestamp <= timestamp {
            return pairs[i].value
        }
    }
    return ""
}
```

#### Complexity:
- **Time Complexity**: `O(N)` for the `Get` operation, where `N` is the number of entries for a key.
- **Space Complexity**: `O(N)`, where `N` is the total number of entries stored across all keys.

---

### Intermediate Approach: Binary Search

#### Intuition:
To optimize the retrieval process, recognize that pairs (value, timestamp) are added in increasing order of timestamps. This allows us to use binary search to find the closest timestamp less than or equal to the requested timestamp.

#### Approach:
- Continue using a map to store keys and a sorted list of pairs.
- Use binary search (`sort.Search`) to efficiently locate the correct value.

#### Code:
```go
import "sort"

type Pair struct {
    value     string
    timestamp int
}

type TimeMap struct {
    store map[string][]Pair
}

func Constructor() TimeMap {
    return TimeMap{store: make(map[string][]Pair)}
}

func (this *TimeMap) Set(key string, value string, timestamp int) {
    this.store[key] = append(this.store[key], Pair{value, timestamp})
}

func (this *TimeMap) Get(key string, timestamp int) string {
    pairs, found := this.store[key]
    if !found {
        return ""
    }
    i := sort.Search(len(pairs), func(i int) bool {
        return pairs[i].timestamp > timestamp
    })
    if i > 0 {
        return pairs[i-1].value
    }
    return ""
}
```

#### Complexity:
- **Time Complexity**: `O(log N)` for the `Get` operation due to binary search, where `N` is the number of entries for a key.
- **Space Complexity**: `O(N)`, where `N` is the total number of entries stored across all keys.

---

### Optimal Approach: HashMap with Sorted List

#### Intuition:
Enhancing the intermediate approach using binary search efficiently retrieves the required data. Further optimizations in retrieval times are not feasible, given the need for a sorted list to perform lookups.

#### Approach:
The optimal approach does not differ from the intermediate approach because we have already achieved logarithmic time complexity for retrieval, which is efficient given the constraints.

The code for the Intermediate and Optimal approaches are the same, as we have reached an optimal balance between time and space complexities.

#### Complexity:
- **Time Complexity**: `O(log N)` for the `Get` operation with binary search, where `N` is the number of entries for a key.
- **Space Complexity**: `O(N)`, where `N` is the total number of entries stored across all keys. 

Continue using the previous `TimeMap` code as it represents the optimal solution.

