# 1834. [Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approach 1: Priority Queue for Task Scheduling

### Solution
```go
// Time Complexity: O(n * log(n)), where n is the number of tasks
// Space Complexity: O(n), for the priority queue and task storage
import (
    "container/heap"
    "sort"
)

type Task struct {
    enqueueTime  int
    processingTime int
    index        int
}

type TaskPQ []Task

func (pq TaskPQ) Len() int { return len(pq) }

func (pq TaskPQ) Less(i, j int) bool {
    if pq[i].processingTime == pq[j].processingTime {
        return pq[i].index < pq[j].index
    }
    return pq[i].processingTime < pq[j].processingTime
}

func (pq TaskPQ) Swap(i, j int) { pq[i], pq[j] = pq[j], pq[i] }

func (pq *TaskPQ) Push(x interface{}) {
    *pq = append(*pq, x.(Task))
}

func (pq *TaskPQ) Pop() interface{} {
    old := *pq
    n := len(old)
    task := old[n-1]
    *pq = old[0 : n-1]
    return task
}

func getOrder(tasks [][]int) []int {
    n := len(tasks)
    result := make([]int, n)
    extendedTasks := make([]Task, n)

    // Add task index and other details for sorting
    for i := 0; i < n; i++ {
        extendedTasks[i] = Task{
            enqueueTime:  tasks[i][0],
            processingTime: tasks[i][1],
            index:        i,
        }
    }

    // Sort tasks by enqueue time
    sort.Slice(extendedTasks, func(i, j int) bool {
        return extendedTasks[i].enqueueTime < extendedTasks[j].enqueueTime
    })

    pq := &TaskPQ{}
    heap.Init(pq)
    time, resultIndex, taskIndex := 0, 0, 0

    for resultIndex < n {
        // Add all tasks available at the current time to the heap
        for taskIndex < n && extendedTasks[taskIndex].enqueueTime <= time {
            heap.Push(pq, extendedTasks[taskIndex])
            taskIndex++
        }

        if pq.Len() == 0 { // No available tasks, move to the next available time
            time = extendedTasks[taskIndex].enqueueTime
            continue
        }

        // Get and process the task with the smallest processing time (or smallest index)
        currentTask := heap.Pop(pq).(Task)
        result[resultIndex] = currentTask.index
        resultIndex++
        time += currentTask.processingTime
    }

    return result
}
```

