# Leetcode Problem: [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming with Priority Queue](#dynamic-programming-with-priority-queue)
3. [Dynamic Programming with Deque for Optimal Complexity](#optimal-dynamic-programming-with-deque)

---

## Brute Force Approach

### Intuition

In the brute force approach, from each element, we try to jump to all possible positions in the range `[i+1, i+k]`, and keep track of the maximum score that can be obtained. This method uses recursion and will explore every path which leads to an exponential time complexity.

### Time Complexity

- **Time:** \(O(n^k)\)
- **Space:** \(O(n)\) (due to recursion stack)

### Code

```go
func maxResult(nums []int, k int) int {
    n := len(nums)
    dp := make([]int, n)
    
    // Helper function for recursion
    var dfs func(index int) int
    dfs = func(index int) int {
        if index == n-1 {
            return nums[n-1]
        }
        
        if dp[index] != 0 {
            return dp[index]
        }
        
        maxScore := math.MinInt32
        for i := 1; i <= k && index+i < n; i++ {
            maxScore = max(maxScore, dfs(index+i))
        }
        
        dp[index] = nums[index] + maxScore
        return dp[index]
    }
    
    return dfs(0)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

---

## Dynamic Programming with Priority Queue

### Intuition

By using dynamic programming and a priority queue (max-heap), we can maintain a window of size `k` for optimal solutions. The priority queue allows us to always access the maximum score quickly for the previous `k` steps.

### Time Complexity

- **Time:** \(O(n \log k)\) (Due to priority queue operations)
- **Space:** \(O(n)\) 

### Code

```go
import (
    "container/heap"
)

func maxResult(nums []int, k int) int {
    n := len(nums)
    dp := make([]int, n)

    pq := &PriorityQueue{}
    heap.Init(pq)
    dp[0] = nums[0]
    heap.Push(pq, &Item{value: dp[0], index: 0})
    
    for i := 1; i < n; i++ {
        // Remove elements not in the window
        for pq.Len() > 0 && pq.Top().index < i-k {
            heap.Pop(pq)
        }
        
        // dp[i] is the current value plus max heaped value
        dp[i] = nums[i] + pq.Top().value
        heap.Push(pq, &Item{value: dp[i], index: i})
    }
    
    return dp[n-1]
}

type Item struct {
    value int
    index int
}

type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
    return pq[i].value > pq[j].value
}

func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
}

func (pq *PriorityQueue) Push(x interface{}) {
    item := x.(*Item)
    *pq = append(*pq, item)
}

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    *pq = old[0 : n-1]
    return item
}

func (pq PriorityQueue) Top() *Item {
    return pq[0]
}
```

---

## Optimal Dynamic Programming with Deque

### Intuition

To achieve optimal time complexity, we use a double-ended queue (deque). The deque will maintain indices that help us apply the sliding window technique for dynamic programming. The deque ensures we are always considering the best possible candidate for jumping within the last `k` entries, while removing any indices that are out of range or that would not result in a maximal score.

### Time Complexity

- **Time:** \(O(n)\)
- **Space:** \(O(n)\)

### Code

```go
func maxResult(nums []int, k int) int {
    n := len(nums)
    dp := make([]int, n)
    dq := make([]int, 0)
    
    dp[0] = nums[0]
    dq = append(dq, 0)
    
    for i := 1; i < n; i++ {
        // Pop elements from dq which are out of the range of k
        if dq[0] < i-k {
            dq = dq[1:]
        }
        
        // The current dp[i] is based on the maximum dp in the deque
        dp[i] = nums[i] + dp[dq[0]]
        
        // Maintain the deque, remove elements which are not better options
        for len(dq) > 0 && dp[i] >= dp[dq[len(dq)-1]] {
            dq = dq[:len(dq)-1]
        }
        
        // Add current element index to the deque
        dq = append(dq, i)
    }
    
    return dp[n-1]
}
```

This approach ensures that each element of `nums` is processed in amortized constant time, giving us optimal performance for real-time applications.

