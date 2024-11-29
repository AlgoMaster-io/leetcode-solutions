# 1834. [Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approach 1: Priority Queue for Task Scheduling

### Solution
```cpp
// Time Complexity: O(n * log(n)), where n is the number of tasks
// Space Complexity: O(n), for the priority queue and task storage
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<int> getOrder(vector<vector<int>>& tasks) {
        int n = tasks.size();
        vector<int> result(n);

        // Add task index to track original order
        vector<vector<int>> extendedTasks(n, vector<int>(3));
        for (int i = 0; i < n; i++) {
            extendedTasks[i][0] = tasks[i][0]; // enqueue time
            extendedTasks[i][1] = tasks[i][1]; // processing time
            extendedTasks[i][2] = i;           // index
        }

        // Sort tasks by enqueue time
        sort(extendedTasks.begin(), extendedTasks.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] < b[0];
        });

        // Min-heap to process tasks by processing time, then by index
        auto cmp = [](const vector<int>& a, const vector<int>& b) {
            return a[1] == b[1] ? a[2] > b[2] : a[1] > b[1];
        };

        priority_queue<vector<int>, vector<vector<int>>, decltype(cmp)> pq(cmp);

        long time = 0;
        int resultIndex = 0, taskIndex = 0;

        while (resultIndex < n) {
            // Add all tasks available at the current time to the heap
            while (taskIndex < n && extendedTasks[taskIndex][0] <= time) {
                pq.push(extendedTasks[taskIndex]);
                taskIndex++;
            }

            if (pq.empty()) {
                // If no tasks are available, jump to the next available task
                time = extendedTasks[taskIndex][0];
                continue;
            }

            // Process the task with the smallest processing time (or smallest index)
            vector<int> currentTask = pq.top();
            pq.pop();
            result[resultIndex++] = currentTask[2];
            time += currentTask[1]; // Increment time by the processing time
        }

        return result;
    }
};
```

