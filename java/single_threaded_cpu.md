# 1834. [Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approach 1: Priority Queue for Task Scheduling

### Solution
```java
// Time Complexity: O(n * log(n)), where n is the number of tasks
// Space Complexity: O(n), for the priority queue and task storage
import java.util.Arrays;
import java.util.PriorityQueue;

public class Solution {
    public int[] getOrder(int[][] tasks) {
        int n = tasks.length;
        int[] result = new int[n];
        
        // Add task index to track original order
        int[][] extendedTasks = new int[n][3];
        for (int i = 0; i < n; i++) {
            extendedTasks[i][0] = tasks[i][0]; // enqueue time
            extendedTasks[i][1] = tasks[i][1]; // processing time
            extendedTasks[i][2] = i; // index
        }

        // Sort tasks by enqueue time
        Arrays.sort(extendedTasks, (a, b) -> Integer.compare(a[0], b[0]));

        // Min-heap to process tasks by processing time, then by index
        PriorityQueue<int[]> pq = new PriorityQueue<>(
            (a, b) -> a[1] == b[1] ? Integer.compare(a[2], b[2]) : Integer.compare(a[1], b[1])
        );

        int time = 0, resultIndex = 0, taskIndex = 0;

        while (resultIndex < n) {
            // Add all tasks available at the current time to the heap
            while (taskIndex < n && extendedTasks[taskIndex][0] <= time) {
                pq.add(extendedTasks[taskIndex]);
                taskIndex++;
            }

            if (pq.isEmpty()) {
                // If no tasks are available, jump to the next available task
                time = extendedTasks[taskIndex][0];
                continue;
            }

            // Process the task with the smallest processing time (or smallest index)
            int[] currentTask = pq.poll();
            result[resultIndex++] = currentTask[2];
            time += currentTask[1]; // Increment time by the processing time
        }

        return result;
    }
}
```