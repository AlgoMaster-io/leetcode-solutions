# [Leetcode 502: IPO](https://leetcode.com/problems/ipo/)

## Solutions

- [Brute Force Approach](#brute-force-approach)
- [Greedy Approach with Priority Queue](#greedy-approach-with-priority-queue)

---

### Brute Force Approach

#### Intuition:

In this approach, we'll simulate the process of investing capital in projects while gradually selecting the most profitable project at each step. The basic idea is to find all feasible projects, i.e., projects whose capital requirement is less than or equal to our current available capital, and then invest in the profitable one among them.

#### Approach:

1. Start by initializing the initial capital available.
2. While you still have project opportunities (`k` is greater than 0):
   - Identify all projects that can be funded (i.e., their `capital[i]` is less than or equal to your current capital).
   - If no such project is found, break out of the loop.
   - From the list of feasible projects, choose the one with the maximum profit.
   - Deduct the initial capital and add the profit of the chosen project to your capital.
   - Decrease the number of available project opportunities (`k`) by 1.
3. Return the final capital amount.

#### Code:

```go
func findMaximizedCapital(k int, W int, Profits []int, Capital []int) int {
    n := len(Profits)
    for k > 0 {
        maxProfit := 0
        index := -1
        
        for i := 0; i < n; i++ {
            // Check if the project is feasible with the current capital
            if Capital[i] <= W {
                // Select the project which gives the maximum profit
                if Profits[i] > maxProfit {
                    maxProfit = Profits[i]
                    index = i
                }
            }
        }
        
        // If no feasible project is found, break the loop
        if index == -1 {
            break
        }
        
        // Increase available capital by the profit of the chosen project
        W += maxProfit
        // We chose this project, so it's no longer available
        Capital[index] = int(1e9+1) // Mark it as unavailable
        k--
    }
    return W
}
```

#### Complexity Analysis:

- **Time Complexity**: O(n * k), where n is the number of projects and `k` is the number of projects we are allowed to invest in. In the worst case, we are iterating over all projects `k` times.
- **Space Complexity**: O(1), as we are not using any extra space except some variables.

---

### Greedy Approach with Priority Queue

#### Intuition:

This approach improves the brute force solution by utilizing a priority queue to efficiently access the most profitable project that can be selected with the current capital. The key idea is to maintain two arrays representing projects sorted by capital and profits to perform operations more efficiently.

#### Approach:

1. Pair the projects with their respective capital and map them into tuples.
2. Sort the projects based on their capital requirement.
3. Use a priority queue to keep track of the projects that can be completed with the current capital, sorting them by profit in descending order.
4. Iterate over the number of projects we can choose (`k` times):
    - While there are projects whose capital requirement is less than or equal to the available capital, add their profit to the priority queue.
    - If no projects are available, break the loop.
    - Pop the most profitable project from the priority queue and add its profit to the total available capital.
5. Return the accumulated capital.

#### Code:

```go
import "container/heap"

type Project struct {
    capital int
    profit  int
}

type MaxHeap []int

func (h MaxHeap) Len() int           { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h MaxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MaxHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *MaxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func findMaximizedCapital(k int, W int, Profits []int, Capital []int) int {
    n := len(Profits)
    projects := make([]Project, n)
    for i := 0; i < n; i++ {
        projects[i] = Project{Capital[i], Profits[i]}
    }

    sort.Slice(projects, func(i, j int) bool {
        return projects[i].capital < projects[j].capital
    })

    maxHeap := &MaxHeap{}
    heap.Init(maxHeap)

    current := 0
    for i := 0; i < k; i++ {
        for current < n && projects[current].capital <= W {
            heap.Push(maxHeap, projects[current].profit)
            current++
        }

        if maxHeap.Len() == 0 {
            break
        }

        W += heap.Pop(maxHeap).(int)
    }

    return W
}
```

#### Complexity Analysis:

- **Time Complexity**: O(n log n + k log n). Sorting projects by the capital takes O(n log n) and each operation on the max-heap takes O(log n). In the worst case, we push all projects once (O(n)) and pop `k` times.
- **Space Complexity**: O(n), as we store all profits in a priority queue.

These solutions progressively optimize the method of selecting the most feasible and profitable projects, utilizing sorting and priority queues for efficient selection.

