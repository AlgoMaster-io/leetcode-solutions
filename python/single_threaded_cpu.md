# 1834. [Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approach 1: Priority Queue for Task Scheduling

### Solution
python
```python
# Time Complexity: O(n * log(n)), where n is the number of tasks
# Space Complexity: O(n), for the priority queue and task storage
from heapq import heappush, heappop
from operator import itemgetter

class Solution:
    def getOrder(self, tasks):
        n = len(tasks)
        result = [0] * n
        
        # Add task index to track original order
        extendedTasks = [[tasks[i][0], tasks[i][1], i] for i in range(n)]

        # Sort tasks by enqueue time
        extendedTasks.sort(key=itemgetter(0))

        # Min-heap to process tasks by processing time, then by index
        pq = []
        time, resultIndex, taskIndex = 0, 0, 0

        while resultIndex < n:
            # Add all tasks available at the current time to the heap
            while taskIndex < n and extendedTasks[taskIndex][0] <= time:
                heappush(pq, (extendedTasks[taskIndex][1], extendedTasks[taskIndex][2]))
                taskIndex += 1

            if not pq:
                # If no tasks are available, jump to the next available task
                time = extendedTasks[taskIndex][0]
                continue

            # Process the task with the smallest processing time (or smallest index)
            currentTask = heappop(pq)
            result[resultIndex] = currentTask[1]
            resultIndex += 1
            time += currentTask[0] # Increment time by the processing time

        return result
```

