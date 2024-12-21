# [Leetcode 1834: Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approaches:
1. [Basic Sorting and Iteration](#approach-1-basic-sorting-and-iteration)
2. [Optimized Priority Queue](#approach-2-optimized-priority-queue)

---

## Approach 1: Basic Sorting and Iteration

### Intuition:
The basic idea is to represent tasks with their indices so they can be sorted effectively by when they arrive. Once sorted, we iterate through them and maintain a current time to simulate how tasks would be processed. Whenever the CPU is idle and there are no available tasks to process at that time, the CPU should skip time to the next task.

### Steps:
1. Parse each task into a list that stores the processing time and original index.
2. Sort the list of tasks based on enqueue time as the primary key and original index as a secondary key. 
3. Iterate through the list of tasks, and process each task based on current time, updating current time as tasks are processed sequentially.
4. Use a list to track the order of processed tasks.

### Time Complexity:
- Sorting the tasks costs O(N log N), where N is the number of tasks.
- The iteration over tasks is O(N).

### Space Complexity:
- O(N) is used to store the tasks and the processing order.

```java
public int[] getOrder(int[][] tasks) {
    int n = tasks.length;
    // Add index to tasks before sorting them
    int[][] taskWithIndex = new int[n][3];
    for (int i = 0; i < n; i++) {
        taskWithIndex[i][0] = tasks[i][0]; // enqueue time
        taskWithIndex[i][1] = tasks[i][1]; // processing time
        taskWithIndex[i][2] = i; // original index
    }
    
    // Sort tasks based on the enqueue time and index
    Arrays.sort(taskWithIndex, (a, b) -> a[0] == b[0] ? Integer.compare(a[2], b[2]) : Integer.compare(a[0], b[0]));

    // Simulate the task processing
    int[] processingOrder = new int[n];
    int currentTime = 0; 
    int index = 0; // index for the result array
    PriorityQueue<int[]> taskQueue = new PriorityQueue<>((a, b) -> a[1] == b[1] ? Integer.compare(a[2], b[2]) : Integer.compare(a[1], b[1]));

    int i = 0;
    while (i < n || !taskQueue.isEmpty()) {
        // Load available tasks into the queue if their enqueue time is less than or equal to current time
        while (i < n && taskWithIndex[i][0] <= currentTime) {
            taskQueue.offer(taskWithIndex[i++]);
        }
        
        if (!taskQueue.isEmpty()) {
            // Process the task with shortest processing time
            int[] task = taskQueue.poll();
            currentTime += task[1];
            processingOrder[index++] = task[2];
        } else {
            // Otherwise, move time forward to next task's starting time if the queue is empty
            currentTime = taskWithIndex[i][0];
        }
    }
    
    return processingOrder;
}
```

---

## Approach 2: Optimized Priority Queue

### Intuition:
Rather than iterating over tasks and moving time sequentially, leveraging a priority queue can optimize task processing based on the shortest processing time. This approach directly jumps to the next available task time by using a priority queue which also handles tasks with identical processing times by their original indices.

### Steps:
1. Store the tasks with their indices, similar to the previous approach.
2. Sort the tasks based on enqueue times.
3. Use a priority queue that sorts tasks first by processing time, and then by original index.
4. Iterate over tasks, but using the priority queue to decide processing order.
5. As before, track the order of processed tasks.

### Time Complexity:
- The main operations are sorting and priority queue operations, both handle N tasks, leading to O(N log N) complexity.

### Space Complexity:
- O(N) for storing tasks and queues.

```java
public int[] getOrder(int[][] tasks) {
    int n = tasks.length;
    // Task array with enqueue time, processing time, and original index
    int[][] tasksWithIndex = new int[n][3];
    for (int i = 0; i < n; i++) {
        tasksWithIndex[i] = new int[] { tasks[i][0], tasks[i][1], i };
    }
    
    // Sort tasks by enqueue time and then by original index
    Arrays.sort(tasksWithIndex, (a, b) -> a[0] == b[0] ? Integer.compare(a[2], b[2]) : Integer.compare(a[0], b[0]));

    int currentTime = 0, index = 0;
    int[] result = new int[n];
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] == b[1] ? Integer.compare(a[2], b[2]) : Integer.compare(a[1], b[1]));
    
    int i = 0;
    while (index < n) {
        // Add all available tasks to the priority queue
        while (i < n && tasksWithIndex[i][0] <= currentTime) {
            pq.offer(tasksWithIndex[i]);
            i++;
        }
        
        if (pq.isEmpty()) {
            // If no tasks are available, jump to the next task's time
            currentTime = tasksWithIndex[i][0];
            continue;
        }
        
        // Process the task with the shortest processing time
        int[] task = pq.poll();
        result[index++] = task[2];
        currentTime += task[1];
    }
    
    return result;
}
```

Both approaches efficiently handle the tasks using sorting and priority queuing, emphasizing managing tasks dynamically based on their availability and processing time requirements.

