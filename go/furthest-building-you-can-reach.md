# [Leetcode 1642: Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Min-Heap (Optimal)](#approach-2-min-heap-optimal)

### Approach 1: Brute Force

#### Intuition:
The brute force approach involves iterating over each building and using the available bricks or ladders naively until we can't go further. This involves checking each jump in the heights array and deciding whether to use bricks or ladders based on the current difference.

#### Steps:
1. Start iterating from the first building.
2. For each jump (i.e., `heights[i]` to `heights[i+1]`), compute the difference.
3. If the difference is positive, decide whether to use bricks or ladders.
4. Use ladders when the height difference is too large for bricks.
5. Use bricks when the height difference is manageable.
6. Stop when you can't make it to the next building.

#### Code:

```go
func furthestBuilding(heights []int, bricks int, ladders int) int {
    n := len(heights)
    
    for i := 0; i < n-1; i++ {
        diff := heights[i+1] - heights[i]
        
        if diff > 0 { // Means next building is taller
            if bricks >= diff {
                bricks -= diff
            } else if ladders > 0 {
                ladders-- // Uses a ladder in place of bricks if sufficient bricks are not available
            } else {
                return i // We cannot reach the next building
            }
        }
    }
    
    return n-1 // Able to reach the last building
}
```

#### Complexity Analysis:
- **Time Complexity:** O(n), where n is the number of buildings.
- **Space Complexity:** O(1), no additional space is used apart from the input.

### Approach 2: Min-Heap (Optimal)

#### Intuition:
The optimal approach makes use of a Min-Heap (priority queue) to efficiently decide when to use bricks and when to switch to ladders. Instead of deciding upfront, you simulate the process and use a Min-Heap to track the largest jumps that should ideally use ladders. The smallest of the largest jumps are replaced with bricks first.

#### Steps:
1. Initialize a Min-Heap to keep track of the largest jumps encountered.
2. Iterate through `heights`, computing the difference for each jump.
3. If a jump (difference) is needed, push it onto the Min-Heap.
4. When the Min-Heap consists of more jumps than ladders available, pop the smallest. This operation is tantamount to using bricks for that particular jump.
5. If at any point bricks are insufficient to cover the remaining jumps (those not handled by ladders), return the index of the current building.
6. Continue until all buildings are covered or bricks run out with remaining jumps uncovered by ladders.

#### Code:

```go
import "container/heap"

// Min-Heap implementation for the go library
type MinHeap []int

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func furthestBuilding(heights []int, bricks int, ladders int) int {
    mh := &MinHeap{}
    heap.Init(mh)
    
    for i := 0; i < len(heights)-1; i++ {
        diff := heights[i+1] - heights[i]
        
        if diff > 0 {
            heap.Push(mh, diff)
        }
        
        // If heights we jumped over, outnumber ladders, use bricks
        if mh.Len() > ladders {
            bricks -= heap.Pop(mh).(int)
        }
        
        if bricks < 0 {
            return i // Bricks are insufficient
        }
    }
    
    return len(heights) - 1 // Able to reach the last building
}
```

#### Complexity Analysis:
- **Time Complexity:** O(n log k), where n is the number of buildings, and k is the number of ladders. The log k factor comes from heap operations.
- **Space Complexity:** O(k), due to the space used by the Min-Heap to store the differences equivalent to the number of ladders.

