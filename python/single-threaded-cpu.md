## [Leetcode 1834: Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu)

### Approaches:
1. [Using a List and Sorting](#approach1)
2. [Using a Min-Heap (Priority Queue)](#approach2)

---

## Approach 1: Using a List and Sorting

### Intuition:
The problem can initially be tackled by utilizing a list to store the tasks with their indices, and then sorting the list based on the enqueue time. The solution requires simulating a CPU processing the tasks. By sorting and using conditionals, we can mimic the processing sequence.

- We first record each task’s index because the result requires tasks to be completed in their original order.
- Sort tasks by their enqueue time and then by their processing time in case of a tie.

### Detailed Steps:
1. Store tasks with their original indices.
2. Sort tasks primarily by enqueue time. If two tasks have the same enqueue time, sort by processing time.
3. Initialize time to keep track of current time and a queue to process tasks.
4. Iterate through tasks, pushing them into a queue when the CPU becomes idle or a task became available.
5. Simulate the processing execution and record the order of completion.

### Time Complexity:
- Sorting the tasks costs \(O(n \log n)\).
- Iterating through tasks and simulating their execution is \(O(n)\).
- Total complexity: \(O(n \log n)\).

### Space Complexity:
- Requires extra space for storing tasks and indices: \(O(n)\).

```python
def getOrder(tasks):
    # Storing tasks with original indices
    tasks = [(start, process, i) for i, (start, process) in enumerate(tasks)]
    # Sort tasks by their start time and processing time
    tasks.sort()
    
    # Current time and result order list
    currTime, idx = 0, 0
    result = []
    from heapq import heappush, heappop
    
    # Min-heap (priority queue) to store tasks based on their processing times
    pq = []
    
    # Loop until all tasks are processed
    while idx < len(tasks) or pq:
        # If the queue is empty, jump to the next available task
        if not pq:
            currTime = max(currTime, tasks[idx][0])
        
        # Add all tasks that are now available up to the current time
        while idx < len(tasks) and tasks[idx][0] <= currTime:
            heappush(pq, (tasks[idx][1], tasks[idx][2]))  # (processing_time, original_index)
            idx += 1
        
        # Choose the next task with the minimum processing time
        process_time, original_index = heappop(pq)
        currTime += process_time
        result.append(original_index)
    
    return result
```

---

## Approach 2: Using a Min-Heap (Priority Queue)

### Intuition:
More optimal approach uses a priority queue to more efficiently manage the tasks that are ready to be processed. This method removes the need to continuously sort the list during execution.

- We simulate the single-threaded CPU using a min-heap to efficiently select the shortest task that can be processed at each point in time.

### Detailed Steps:
1. Insert each task along with its index into a list and sort it by enqueue time.
2. Use a min-heap to always have immediate access to the task with the shortest processing time.
3. Track current time and increment it as each task is processed.
4. While there are tasks available to be processed, push them into the heap.
5. Pop the heap to get the task with the minimum processing time.

### Time Complexity:
- Sorting tasks before processing: \(O(n \log n)\).
- Each enqueue/dequeue operation from the heap: \(O(\log n)\).
- Total complexity: \(O(n \log n)\) due to the sorting and heap operations.

### Space Complexity:
- Storing tasks in extra heap: \(O(n)\).

```python
from heapq import heappush, heappop

def getOrder(tasks):
    n = len(tasks)
    indexed_tasks = sorted([(t[0], t[1], i) for i, t in enumerate(tasks)])
    result = []

    min_heap = []
    time = 0
    task_index = 0

    while task_index < n or min_heap:
        if not min_heap:
            time = max(time, indexed_tasks[task_index][0])
        
        while task_index < n and indexed_tasks[task_index][0] <= time:
            heappush(min_heap, (indexed_tasks[task_index][1], indexed_tasks[task_index][2]))
            task_index += 1
        
        process_time, original_index = heappop(min_heap)
        result.append(original_index)
        time += process_time

    return result
```

This approach leverages the efficiency of a min-heap to minimize the processing time associated with finding the next task, hence achieving a more optimal solution.

