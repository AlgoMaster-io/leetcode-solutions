[Leetcode Problem 1834: Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approaches
- [Approach 1: Simulation using Sorting and Priority Queue](#approach-1-simulation-using-sorting-and-priority-queue)

---

### Approach 1: Simulation using Sorting and Priority Queue

#### Intuition
The task is to simulate a CPU execution of tasks based on given rules of enqueue and processing. Each task is defined by its enqueue time and processing time. The CPU should always execute the available task with the shortest processing time, and if multiple tasks have the same processing time, the task with the smaller index should be executed first.

To efficiently solve this problem, we'll use a min-heap (priority queue) to keep track of tasks with the respect of processing time, and we'll sort the tasks according to their enqueue time.

#### Steps
1. First, store the original index of tasks along with their enqueue and processing time. This will help maintain the order when they have the same processing time in the queue.
2. Sort the tasks by their enqueue time. This will help in simulating the time progression.
3. Use a priority queue (min-heap) to store the tasks when they become available for execution. The priority queue will allow us to always get the task with the shortest processing time.
4. Initialize a current time to keep track of the CPU clock.
5. Iterate through each task, using the priority queue to determine which task should be executed next based on the current time.
6. If no task is available to execute, skip the time to the enqueue time of the next task.
7. Execute the task, add it to the result in the execution order, and update the current time to reflect the duration of processed task.

#### Time Complexity
The time complexity of this approach is \(O(n \log n)\), where \(n\) is the number of tasks. This arises because of sorting the tasks and operations with the priority queue.

#### Space Complexity
The space complexity is \(O(n)\) due to storing tasks in additional data structures.

```cpp
#include <vector>
#include <queue>
#include <algorithm>

// Function to get the order of execution of tasks
std::vector<int> getOrder(std::vector<std::vector<int>>& tasks) {
    int n = tasks.size();
    
    // Store task with their original index
    std::vector<std::vector<int>> indexedTasks;
    for (int i = 0; i < n; i++) {
        indexedTasks.push_back({tasks[i][0], tasks[i][1], i});
    }
    
    // Sort tasks by their enqueue time
    std::sort(indexedTasks.begin(), indexedTasks.end());
    
    // Min-heap to process tasks based on processing time
    std::priority_queue<std::pair<int, int>, 
                        std::vector<std::pair<int, int>>, 
                        std::greater<std::pair<int, int>>> pq;
    
    std::vector<int> result;
    long long currTime = 0;
    int index = 0;
    
    // Process all tasks
    while (index < n || !pq.empty()) {
        // Load all tasks that are available by current time
        while (index < n && indexedTasks[index][0] <= currTime) {
            pq.push({indexedTasks[index][1], indexedTasks[index][2]});
            index++;
        }
        
        if (!pq.empty()) {
            // Get the task with the shortest processing time
            auto [processingTime, taskIndex] = pq.top();
            pq.pop();
            
            // Execute the task
            currTime += processingTime;
            result.push_back(taskIndex);
        } else {
            // If no tasks are available, fast-forward time to next task's enqueue time
            if (index < n) {
                currTime = indexedTasks[index][0];
            }
        }
    }
    
    return result;
}
```

This approach efficiently uses sorting and a priority queue to simulate the CPU task scheduling under the given constraints, ensuring the order of execution is optimal. Each important step in the simulation, from loading tasks into the queue to executing them and handling idle times, is carefully managed to align with the problem constraints.

