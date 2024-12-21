# [Leetcode 1882: Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)

## Approaches:

1. [Brute Force using Iteration](#approach-1---brute-force-using-iteration)
2. [Optimized Using Two Priority Queues](#approach-2---optimized-using-two-priority-queues)

### Approach 1 - Brute Force using Iteration

#### Intuition

The brute force approach involves iterating through each server for every task and checking if a server is available. This is a straightforward method focusing on simply finding any available server at each point in time and allocating tasks sequentially. 

1. Use a loop to progress through each task's processing time while manually checking each server's availability by storing a next available time for each.
2. Naive allocation: for each task, choose the next available server without any particular optimization.

#### Implementation

```cpp
#include <vector>
#include <queue>
using namespace std;

vector<int> assignTasks(vector<int>& servers, vector<int>& tasks) {
    int n = servers.size();
    int m = tasks.size();
    vector<int> result(m);
    vector<int> available(n, 0);  // Tracks when each server becomes available

    for (int i = 0, t = 0; i < m; ++i) {
        // Move time t to at least current task time
        t = max(t, i);
        
        // Find server with the least weight that is available
        int chosenServer = -1;
        for (int j = 0; j < n; ++j) {
            if (t >= available[j] && (chosenServer == -1 || servers[j] < servers[chosenServer])) {
                chosenServer = j;  // Select the lightest available server
            }
        }
        
        // Assign the task
        result[i] = chosenServer;
        available[chosenServer] = t + tasks[i];  // Update server's next available time
    }
    
    return result;
}
```

#### Complexity Analysis

- **Time Complexity:** \(O(m \cdot n)\), where \(m\) is the number of tasks and \(n\) is the number of servers. Each task requires an check through all servers.
- **Space Complexity:** \(O(m + n)\) for maintaining the output and server availability.

### Approach 2 - Optimized Using Two Priority Queues

#### Intuition

The optimal approach for solving this problem efficiently involves utilizing two priority queues: one for keeping track of available servers and the other for busy servers.

1. **Available Servers:** A min-heap sorted by priority `(server weight, server index)`. This allows us to efficiently retrieve the lightest server that is available for a task.
2. **Busy Servers:** A min-heap sorted by the time at which they become available again. This allows us to efficiently schedule tasks by checking when a suitable server becomes available.

We progress through each task, and at every task's start time, we move servers that have finished their previous task from the busy list back to available.

#### Implementation

```cpp
#include <vector>
#include <queue>
using namespace std;

vector<int> assignTasks(vector<int>& servers, vector<int>& tasks) {
    int n = servers.size();
    vector<int> result;
    priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<>> available;  // min-heap: (weight, index, server index)
    priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<>> busy;  // min-heap: (end time, weight, server index)

    // Initialize available servers queue
    for (int i = 0; i < n; ++i) {
        available.push({servers[i], {i, i}});
    }

    // Traverse through each task
    int time = 0;
    for (int i = 0; i < tasks.size(); ++i) {
        // Advance time to when next task is available if no server is free yet
        time = max(time, i);

        // Release servers that have completed tasks and move to available
        while (!busy.empty() && busy.top().first <= time) {
            auto [endTime, info] = busy.top(); busy.pop();
            available.push(info);
        }

        // If no server is available, fast forward in time
        if (available.empty()) {
            time = busy.top().first;
            while (!busy.empty() && busy.top().first == time) {
                auto [endTime, info] = busy.top(); busy.pop();
                available.push(info);
            }
        }

        // Assign the current task
        auto [weight, serverInfo] = available.top(); available.pop();
        auto [serverIndex, originalIndex] = serverInfo;
        result.push_back(serverIndex);

        // Schedule the server to be busy until time + task duration
        busy.push({time + tasks[i], {weight, serverIndex}});
    }

    return result;
}
```

#### Complexity Analysis

- **Time Complexity:** \(O((m + n) \log n)\), where \(m\) is the number of tasks, and \(n\) is the number of servers. Each insertion and deletion in the priority queue operates in \(O(\log n)\).
- **Space Complexity:** \(O(n + m)\), since we are storing server states and tasks in the priority queues and a resultant vector.


