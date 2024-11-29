# 1834. [Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approach 1: Priority Queue for Task Scheduling

### Solution
```csharp
// Time Complexity: O(n * log(n)), where n is the number of tasks
// Space Complexity: O(n), for the priority queue and task storage

using System;
using System.Collections.Generic;

public class Solution {
    public int[] GetOrder(int[][] tasks) {
        int n = tasks.Length;
        int[] result = new int[n];
        
        // Add task index to track original order
        int[][] extendedTasks = new int[n][];
        for (int i = 0; i < n; i++) {
            extendedTasks[i] = new int[] { tasks[i][0], tasks[i][1], i }; // enqueue time, processing time, index
        }

        // Sort tasks by enqueue time
        Array.Sort(extendedTasks, (a, b) => a[0].CompareTo(b[0]));

        // Min-heap to process tasks by processing time, then by index
        var pq = new PriorityQueue<int[], int>(Comparer<int>.Create((a, b) => {
            int comparison = extendedTasks[a][1].CompareTo(extendedTasks[b][1]);
            return comparison != 0 ? comparison : extendedTasks[a][2].CompareTo(extendedTasks[b][2]);
        }));

        int time = 0, resultIndex = 0, taskIndex = 0;

        while (resultIndex < n) {
            // Add all tasks available at the current time to the heap
            while (taskIndex < n && extendedTasks[taskIndex][0] <= time) {
                pq.Enqueue(taskIndex, taskIndex);
                taskIndex++;
            }

            if (pq.Count == 0) {
                // If no tasks are available, jump to the next available task
                time = extendedTasks[taskIndex][0];
                continue;
            }

            // Process the task with the smallest processing time (or smallest index)
            int[] currentTask = extendedTasks[pq.Dequeue()];
            result[resultIndex++] = currentTask[2];
            time += currentTask[1]; // Increment time by the processing time
        }

        return result;
    }
}
```


