# [Leetcode 1834: Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approaches
1. [Basic Approach: Sort and Simulate Completion](#basic-approach-sort-and-simulate-completion)
2. [Optimized Approach: Min-Heap for Efficient Task Selection](#optimized-approach-min-heap-for-efficient-task-selection)

---

### Basic Approach: Sort and Simulate Completion

**Intuition**:
The problem can be simplified by first sorting the tasks based on their enqueue time, which allows us to simulate the processing of tasks as they arrive. Then, using a sort operation, we can schedule tasks whenever the CPU is free.

**Approach**:
1. Pair each task with its original index for output purposes.
2. Sort the tasks on the basis of enqueue time. When enqueue times are the same, sort based on processing times.
3. Use a loop to simulate task processing:
   - Keep track of the current time as tasks are processed.
   - Once the CPU is free, select the next task in line.
   - Process tasks until all are completed.
4. Store the result in terms of the original indices after determining the processing order.

**Code**:
```go
func getOrder(tasks [][]int) []int {
    type Task struct {
        startTime, duration, index int
    }
    
    // Prepare slices with task details indexed for sorting
    taskList := make([]Task, len(tasks))
    for i, task := range tasks {
        taskList[i] = Task{
            startTime: task[0],
            duration:  task[1],
            index:     i,
        }
    }
    
    // Sort based on start time, and then duration
    sort.Slice(taskList, func(i, j int) bool {
        if taskList[i].startTime == taskList[j].startTime {
            return taskList[i].duration < taskList[j].duration
        }
        return taskList[i].startTime < taskList[j].startTime
    })
    
    // Order of tasks to be returned
    result := make([]int, 0, len(tasks))
    time := 0
    i := 0
    
    // Simulate CPU processing
    for len(result) < len(tasks) {
        if i < len(tasks) && time < taskList[i].startTime && len(result) == 0 {
            time = taskList[i].startTime
        }
        
        for i < len(tasks) && taskList[i].startTime <= time {
            result = append(result, taskList[i].index)
            time += taskList[i].duration
            i++
        }
    }
    
    return result
}
```

**Time Complexity**: O(n log n), mainly due to sorting.
**Space Complexity**: O(n), for storage of task information with indices.

---

### Optimized Approach: Min-Heap for Efficient Task Selection

**Intuition**:
Using a min-heap (priority queue) allows us to efficiently select the task that should be executed next based on the shortest processing time and enqueue time. This allows for more efficient scheduling.

**Approach**:
1. Pair each task with its original index for output purposes.
2. Sort the tasks based on their enqueue time.
3. Use a min-heap to schedule tasks:
   - Push tasks into the heap as they become available based on the CPU's current time.
   - Always process the task that has the smallest processing time.
   - If the heap is empty but tasks remain, jump the time to the next task's start time.
4. Store the result in terms of the original indices.

**Code**:
```go
import (
    "container/heap"
    "sort"
)

type Task struct {
    enqueueTime, processingTime, index int
}

// Implementing heap interface for Task priority based on processingTime and index
type TaskHeap []Task

func (h TaskHeap) Len() int           { return len(h) }
func (h TaskHeap) Less(i, j int) bool {
    if h[i].processingTime == h[j].processingTime {
        return h[i].index < h[j].index
    }
    return h[i].processingTime < h[j].processingTime
}
func (h TaskHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *TaskHeap) Push(x interface{}) {
    *h = append(*h, x.(Task))
}
func (h *TaskHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func getOrder(tasks [][]int) []int {
    taskList := make([]Task, len(tasks))
    for i, task := range tasks {
        taskList[i] = Task{task[0], task[1], i}
    }
    
    // Sort tasks based on enqueue time
    sort.Slice(taskList, func(i, j int) bool {
        return taskList[i].enqueueTime < taskList[j].enqueueTime
    })
    
    var results []int
    minHeap := &TaskHeap{}
    heap.Init(minHeap)
    time, index := 0, 0
    
    for len(results) < len(tasks) {
        // Push all tasks into heap that can be executed at this time
        for index < len(taskList) && taskList[index].enqueueTime <= time {
            heap.Push(minHeap, taskList[index])
            index++
        }
        
        if minHeap.Len() == 0 {
            // If heap is empty, move time to the next task enqueueTime
            if index < len(taskList) {
                time = taskList[index].enqueueTime
            }
            continue
        }
        
        // Pop task with the smallest processing time
        currentTask := heap.Pop(minHeap).(Task)
        results = append(results, currentTask.index)
        time += currentTask.processingTime
    }
    
    return results
}
```

**Time Complexity**: O(n log n), due to sorting and heap operations.
**Space Complexity**: O(n), for task storage and heap.

By efficiently utilizing a min-heap, we significantly reduce the time complexity involved in scheduling and selecting tasks, making this approach optimal for larger datasets.

