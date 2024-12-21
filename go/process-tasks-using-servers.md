# [Leetcode 1882: Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach Using Priority Queues](#optimized-approach-using-priority-queues)

---

## Brute Force Approach

### Intuition

The brute force approach involves assigning tasks to servers by simply iterating over all available servers each time a task is to be processed. We will keep track of the servers and the times they become free. This way, for each task, we look for the first available server or the server that will become free the earliest. However, this doesn't efficiently handle large inputs due to its time complexity.

### Approach

1. Create a structure to hold the server's weight and index and maintain a list of servers.
2. Use two arrays:
   - One for tracking the next available time for each server.
   - Another to mark which task is assigned to which server.
3. For each task, iterate over all servers to find the server with the minimum weight that is available to process the task.
4. Assign the task to the selected server and update the server's next available time.
5. Repeat until all tasks are assigned.

### Complexity Analysis

- **Time Complexity**: O(n * m), where n is the number of tasks and m is the number of servers. For each task, we iterate over all servers to find the best one to assign.
- **Space Complexity**: O(m), primarily for storing server states.

### Code

```go
func assignTasks(servers []int, tasks []int) []int {
    n := len(servers)
    m := len(tasks)
    assigned := make([]int, m)
    nextAvailable := make([]int, n)

    for i := 0; i < m; i++ {
        minIndex := -1
        for j := 0; j < n; j++ {
            if nextAvailable[j] <= i {
                if minIndex == -1 || servers[j] < servers[minIndex] || 
                   (servers[j] == servers[minIndex] && j < minIndex) {
                    minIndex = j
                }
            }
        }
        assigned[i] = minIndex
        nextAvailable[minIndex] = i + tasks[i]
    }

    return assigned
}
```

---

## Optimized Approach Using Priority Queues

### Intuition

To optimize the task assignment, we can leverage two priority queues (or min-heaps):
1. One for available servers sorted by their weight and index.
2. One for busy servers sorted by the time they will become available.

This allows us to efficiently determine which server to assign a task to as the tasks are processed sequentially over time.

### Approach

1. Initialize two priority queues:
   - One (`available`) for storing servers that are currently available.
   - One (`busy`) for storing currently busy servers along with the time they will be free.
2. For each task at a particular timestamp:
   - Move all servers from `busy` to `available` if they have become available.
   - Assign the task to the server at the top of the `available` queue.
   - Update the `busy` queue with this server's next free time.
3. Repeat until all tasks are processed.

### Complexity Analysis

- **Time Complexity**: O((n + m) log m), where n is the number of tasks, and m is the number of servers. Logarithmic complexities come from heap operations.
- **Space Complexity**: O(m), for storing the server availability states in the priority queues.

### Code

```go
import (
    "container/heap"
    "sort"
)

type Server struct {
    weight int
    index  int
    freeTime int
}

type ServerHeap []Server

func (h ServerHeap) Len() int { return len(h) }
func (h ServerHeap) Less(i, j int) bool {
    if h[i].freeTime == h[j].freeTime {
        if h[i].weight == h[j].weight {
            return h[i].index < h[j].index
        }
        return h[i].weight < h[j].weight
    }
    return h[i].freeTime < h[j].freeTime
}

func (h *ServerHeap) Push(x interface{}) {
    *h = append(*h, x.(Server))
}

func (h *ServerHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func (h ServerHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }

func assignTasks(servers []int, tasks []int) []int {
    available := &ServerHeap{}
    busy := &ServerHeap{}
    assigned := make([]int, len(tasks))

    heap.Init(available)
    heap.Init(busy)

    for index, weight := range servers {
        heap.Push(available, Server{weight, index, 0})
    }

    currentTime := 0

    for i, duration := range tasks {
        currentTime = max(currentTime, i)
        
        for busy.Len() > 0 && (*busy)[0].freeTime <= currentTime {
            server := heap.Pop(busy).(Server)
            heap.Push(available, Server{server.weight, server.index, 0})
        }

        if available.Len() > 0 {
            server := heap.Pop(available).(Server)
            assigned[i] = server.index
            heap.Push(busy, Server{server.weight, server.index, currentTime + duration})
        } else {
            server := heap.Pop(busy).(Server)
            currentTime = server.freeTime
            assigned[i] = server.index
            heap.Push(busy, Server{server.weight, server.index, currentTime + duration})
        }
    }

    return assigned
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

This optimized solution efficiently handles large inputs by using heaps to minimize unnecessary operations, providing a scalable approach to the task-server assignment problem.

