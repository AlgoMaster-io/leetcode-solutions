# [1834. Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Efficient Approach Using Priority Queue](#efficient-approach-using-priority-queue)

### Brute Force Approach

**Intuition:**

The idea is to simulate processing each task. We initially sort the tasks by their starting times so that we can sequentially pick the next available task. For each time unit, we check if any task can begin. Once found, we execute the task with the smallest processing time to minimize waiting time.

**Steps:**
1. Add indexes to the tasks for later retrieval.
2. Sort tasks based on starting time. For the same start time, preserve their input order.
3. Use a while loop to simulate the passage of time. At each time `t`, select the tasks that can start (i.e., `task availableTime = t`). Select and execute the one with the smallest processing time.
  
**Code:**

```typescript
function getOrder(tasks: number[][]): number[] {
    // Step 1: Sort tasks based on the available time (and then by processing time).
    let tasksWithIndex = tasks.map((task, index) => [...task, index]);
    tasksWithIndex.sort((a, b) => a[0] - b[0]);

    let currentTime = 0;
    const result: number[] = [];
    const availableTasks: [number, number][] = []; // Stores pairs of <processingTime, index>

    let index = 0;

    while (result.length < tasks.length) {
        // Add all tasks that have become available up to the current time
        while (index < tasksWithIndex.length && tasksWithIndex[index][0] <= currentTime) {
            // Push tuple (-processing time, index). We use a min-heap behavior based on processing time.
            availableTasks.push([tasksWithIndex[index][1], tasksWithIndex[index][2]]);
            index++;
        }

        // Sort available tasks by processing time and index
        availableTasks.sort((a, b) => a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]);

        if (availableTasks.length > 0) {
            // Pop the task with the smallest processing time (ties broken by index)
            const [taskIndex, rawIndex] = availableTasks.shift()!;
            result.push(rawIndex);
            currentTime += taskIndex; // Simulate processing time
        } else {
            // If no tasks are available, jump to the next task arrival time
            currentTime = tasksWithIndex[index][0];
        }
    }

    return result;
}
```

**Time Complexity:** O(n^2 log n) due to sorting tasks within the loop.

**Space Complexity:** O(n) for storing tasks.

### Efficient Approach Using Priority Queue

**Intuition:**

The brute force solution can be made more efficient by using a priority queue (min-heap) to automatically handle tasks based on their processing time. Here’s how:

1. We first sort tasks based on their start times.
2. Utilize a min-heap to track and quickly identify the task with the shortest processing time.
3. Simulate the operations of a CPU to pick tasks from the heap when the current time matches task availability.

**Steps:**
1. Sort the tasks by start times, with secondary sorting based on index for the same available time.
2. Utilize a priority queue to store and retrieve tasks based on their processing time.
3. Use a loop to iterate over time slots [0...∞) until all tasks are processed: Add all tasks that have arrived and select one from the priority queue.
4. Execute the selected task, increase the time counter, and add the result.

**Code:**

```typescript
function getOrder(tasks: number[][]): number[] {
    // Sort the tasks by their start time, and use the index for ordering equivalent start times
    let indexedTasks = tasks.map((task, index) => [...task, index]);
    indexedTasks.sort((a, b) => a[0] - b[0] || a[2] - b[2]);

    let currentTime = 0;
    let index = 0;
    const result: number[] = [];
    const availableTasks = new MinPriorityQueue({ compare: (a, b) => a[0] === b[0] ? a[1] - b[1] : a[0] - b[0] });

    while (result.length < tasks.length) {
        // Add all tasks that have become available by `currentTime`
        while (index < indexedTasks.length && indexedTasks[index][0] <= currentTime) {
            availableTasks.enqueue([indexedTasks[index][1], indexedTasks[index][2]]);
            index++;
        }

        if (!availableTasks.isEmpty()) {
            // Get and remove the task from the priority queue
            const [processingTime, rawIndex] = availableTasks.dequeue();
            result.push(rawIndex);
            currentTime += processingTime; // Simulate task processing
        } else {
            // If no tasks are available, jump to the next available task time
            currentTime = indexedTasks[index][0];
        }
    }

    return result;
}
```

**Time Complexity:** O(n log n) due to sorting and efficient priority queue operations.

**Space Complexity:** O(n) needed for the priority queue and resultant storage.

