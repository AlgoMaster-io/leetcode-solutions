# [Leetcode 621: Task Scheduler](https://leetcode.com/problems/task-scheduler/)

## Approaches
- [Approach 1: Simulation with Priority Queue](#approach-1-simulation-with-priority-queue)
- [Approach 2: Optimized Calculation with Counting](#approach-2-optimized-calculation-with-counting)

### Approach 1: Simulation with Priority Queue

**Intuition:**

In this approach, we attempt to simulate the scheduling of tasks using a priority queue, which helps in selecting the most frequent tasks to execute. We aim to fill the cycle of intervals (`n`) with available tasks in order to minimize idle time.

**Steps:**

1. Calculate the frequency of each task.
2. Use a max-heap (priority queue) to store tasks based on their frequencies.
3. Iteratively fill the interval with tasks selected from the priority queue, ensuring each task follows the `n` cooldown restriction.
4. If fewer tasks than the cycle are available, idle time is added to the schedule.
5. Repeat until all tasks are completed.

**Code:**

```cpp
#include <vector>
#include <unordered_map>
#include <queue>

int leastInterval(std::vector<char>& tasks, int n) {
    std::unordered_map<char, int> taskCount;
    for (char task : tasks) {
        taskCount[task]++;
    }

    std::priority_queue<int> pq;
    for (auto& entry : taskCount) {
        pq.push(entry.second);
    }

    int intervals = 0;

    while (!pq.empty()) {
        int cycle = n + 1;
        std::vector<int> tempList;
        
        // Try to execute as many as possible within one cycle
        while (cycle > 0 && !pq.empty()) {
            int topTask = pq.top();
            pq.pop();
            tempList.push_back(topTask - 1);
            cycle--;
            intervals++;
        }
        
        // Re-insert remaining tasks back into the queue
        for (int count : tempList) {
            if (count > 0) {
                pq.push(count);
            }
        }
        
        // If there's no task left, no need to add idle times
        if (pq.empty()) {
            break;
        }

        // Fill the rest of the cycle with idle if necessary
        intervals += cycle;  // Only adds if more cycle time remains
    }

    return intervals;
}
```

**Complexity Analysis:**

- Time Complexity: \(O(N \cdot \log N)\), where \(N\) is the number of tasks. This is due to the max-heap operations.
- Space Complexity: \(O(1)\), as the number of possible tasks \(A-Z\) is fixed (constant space).

### Approach 2: Optimized Calculation with Counting

**Intuition:**

This approach focuses on calculating the minimum intervals directly derived from the frequency of tasks. The most time-consuming (frequent) task group determines the necessary intervals due to their cooldown requirements.

**Steps:**

1. Calculate the frequency of each task.
2. Determine the maximum frequency of any task.
3. Calculate the number of tasks that have this maximum frequency.
4. Compute the required interval using the formula \((\text{max\_count} - 1) \times (n + 1) + \text{num\_max\_tasks}\). 
5. Choose the maximum between computed interval and the total number of tasks to account for cases when there are enough distinct tasks to fill cycles without idles.

**Code:**

```cpp
#include <vector>
#include <algorithm>

int leastInterval(std::vector<char>& tasks, int n) {
    std::vector<int> freq(26, 0);
    for (char task : tasks) {
        freq[task - 'A']++;
    }

    // Find the task with the maximum frequency
    int max_count = *std::max_element(freq.begin(), freq.end());
    // Count how many tasks have this maximum frequency
    int num_max_tasks = std::count(freq.begin(), freq.end(), max_count);

    // Calculate minimum length needed
    int min_length = (max_count - 1) * (n + 1) + num_max_tasks;

    return std::max(min_length, static_cast<int>(tasks.size()));
}
```

**Complexity Analysis:**

- Time Complexity: \(O(N + 26)\), which simplifies to \(O(N)\), where \(N\) is the number of tasks, due to constant size array operations for tasks A-Z.
- Space Complexity: \(O(1)\), due to the fixed size of the frequency array.

This optimized approach efficiently calculates the necessary task intervals, leveraging the task frequency directly to estimate minimum execution time.

