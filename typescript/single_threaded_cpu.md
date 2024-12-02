# 1834. [Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approach 1: Priority Queue for Task Scheduling

### Solution
typescript
```typescript
// Time Complexity: O(n * log(n)), where n is the number of tasks
// Space Complexity: O(n), for the priority queue and task storage

function getOrder(tasks: number[][]): number[] {
    const n = tasks.length;
    const result: number[] = new Array(n);
    
    // Add task index to track original order
    const extendedTasks: number[][] = tasks.map((task, index) => [task[0], task[1], index]); // [enqueue time, processing time, index]

    // Sort tasks by enqueue time
    extendedTasks.sort((a, b) => a[0] - b[0]);

    // Min-heap to process tasks by processing time, then by index
    const pq = new MinPriorityQueue({ 
        compare: (a: number[], b: number[]) => a[1] === b[1] ? a[2] - b[2] : a[1] - b[1]
    });

    let time = 0, resultIndex = 0, taskIndex = 0;

    while (resultIndex < n) {
        // Add all tasks available at the current time to the heap
        while (taskIndex < n && extendedTasks[taskIndex][0] <= time) {
            pq.enqueue(extendedTasks[taskIndex]);
            taskIndex++;
        }

        if (pq.isEmpty()) {
            // If no tasks are available, jump to the next available task
            time = extendedTasks[taskIndex][0];
            continue;
        }

        // Process the task with the smallest processing time (or smallest index)
        const currentTask = pq.dequeue();
        result[resultIndex++] = currentTask[2];
        time += currentTask[1]; // Increment time by the processing time
    }

    return result;
}
```

Note: `MinPriorityQueue` is assumed to be a utility class available in the environment, similar to the `PriorityQueue` in Java. If not available, a min-heap implementation using an array could be used.

