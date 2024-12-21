# [Leetcode 1834: Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approaches
1. [Sorting and Priority Queue Approach](#sorting-and-priority-queue-approach)

---

### Sorting and Priority Queue Approach

To solve the problem of simulating a Single-Threaded CPU that processes tasks in a specific order, we can utilize a clever combination of sorting and a priority queue (min-heap). The main challenge is to ensure that tasks are processed in the correct order according to the given criteria: tasks with the earliest `enqueue` time should be considered first, and among tasks that can be processed, we should pick the one with the shortest processing time.

#### Intuition

1. **Sort Tasks by Enqueue Time**: 
   - First, we sort the tasks based on their enqueue time. In addition to enqueue time, also track their original index because we'll need to return results in the input order for tasks that finish execution.
   
2. **Use a Min-Heap for Processing**:
   - A priority queue (min-heap) is used to store tasks that are eligible for processing. The heap is ordered by processing time, and for tasks with identical processing times, we use their index to break ties.
   
3. **Simulate the CPU**:
   - We simulate the passage of time by iterating through the sorted list of tasks and leveraging the heap to pick the appropriate task for execution.
   - If no tasks can be processed (i.e., the CPU is idle), fast-forward the CPU time to the enqueue time of the next available task.

#### Steps

1. **Sort the Data**: Prepare a sorted list of tasks alongside their original indices.
2. **Initialize Priority Queue**: Use a priority queue to determine which task to execute based on processing time and tiebreaker (index).
3. **Iterate Over Tasks**: Use a loop to iterate over tasks, updating the current time and executing tasks from the priority queue as possible.
4. **Output the Order**: Keep track of the executed order using the task indices and finally return this order.

### C# Code Implementation

```csharp
public int[] GetOrder(int[][] tasks)
{
    // Step 1: Annotate tasks with their original indices and sort them by enqueueTime
    int n = tasks.Length;
    int[][] sortedTasks = new int[n][];
    for (int i = 0; i < n; i++)
    {
        sortedTasks[i] = new int[] { tasks[i][0], tasks[i][1], i };
    }
    Array.Sort(sortedTasks, (a, b) => a[0].CompareTo(b[0]));

    // Step 2: Use a priority queue (min-heap) ordered by processing time and index
    var taskQueue = new SortedSet<(int processingTime, int index)>();
    int[] result = new int[n];
    int time = 0;
    int taskIndex = 0;
    int resultIndex = 0;

    while (taskIndex < n || taskQueue.Count > 0)
    {
        // Step 3: Enqueue all tasks whose enqueueTime <= current time
        while (taskIndex < n && sortedTasks[taskIndex][0] <= time)
        {
            taskQueue.Add((sortedTasks[taskIndex][1], sortedTasks[taskIndex][2]));
            taskIndex++;
        }

        // Step 4: If the taskQueue is not empty, process the task with the smallest processing time
        if (taskQueue.Count > 0)
        {
            var currentTask = taskQueue.Min;
            taskQueue.Remove(currentTask);
            time += currentTask.processingTime;
            result[resultIndex++] = currentTask.index;
        }
        else
        {
            // Step 5: If no tasks can be processed, fast forward time to the next available task's enqueueTime
            if (taskIndex < n)
            {
                time = sortedTasks[taskIndex][0];
            }
        }
    }

    return result;
}
```

#### Complexity Analysis

- **Time Complexity**:  
  - The sorting of tasks takes O(n log n) time.
  - Each task is inserted and removed from the heap exactly once, taking O(log n) per operation. Thus, the heap operations take O(n log n) overall.
  - Hence, the total time complexity is O(n log n).

- **Space Complexity**: 
  - Sorting requires O(n) additional space for storing task data structures.
  - The priority queue can also hold up to `n` tasks simultaneously, so it uses O(n) space.
  - Therefore, the space complexity is O(n).

