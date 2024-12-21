# [Leetcode 1882: Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)

## Approaches:
- [Approach 1: Brute Force Method](#approach-1-brute-force-method)
- [Approach 2: Priority Queues for Available and Busy Servers (Optimal)](#approach-2-priority-queues-for-available-and-busy-servers-optimal)

---

## Approach 1: Brute Force Method

### Intuition:
In this approach, we try to simulate the server-task assignment by iterating over each task and assigning the available server with the smallest index or the lightest weight at each time point. This involves checking each server's availability status at every task arrival time which yields a brute force O(n * m) complexity, where `n` is the number of tasks and `m` is the number of servers.

### Steps:
1. Initialize an array to track the next available time for each server.
2. For each task, iterate over all servers to find the best available server.
3. Assign the task to the best found server and update its availability.

Here's the code implementation:

```csharp
public int[] AssignTasks(int[] servers, int[] tasks) {
    int n = servers.Length;
    int m = tasks.Length;
    int[] result = new int[m];
    int[] availableTimes = new int[n];

    for (int taskIndex = 0; taskIndex < m; taskIndex++) {
        int bestServer = -1;

        // Select the best server for the current task
        for (int serverIndex = 0; serverIndex < n; serverIndex++) {
            if (availableTimes[serverIndex] <= taskIndex) { // server is free
                if (bestServer == -1 || servers[serverIndex] < servers[bestServer] || 
                    (servers[serverIndex] == servers[bestServer] && serverIndex < bestServer)) {
                    bestServer = serverIndex;
                }
            }
        }

        // Assign task to the best server found
        result[taskIndex] = bestServer;
        availableTimes[bestServer] = taskIndex + tasks[taskIndex]; // update availability
    }
    return result;
}
```

### Complexity Analysis:
- **Time Complexity:** O(n * m), where `n` is the number of servers and `m` is the number of tasks.
- **Space Complexity:** O(n), as we are storing the availability status of the servers.

---

## Approach 2: Priority Queues for Available and Busy Servers (Optimal)

### Intuition:
To optimize the process, we can use two priority queues: one for available servers and one for busy servers. The available servers are prioritized by weight and indices when they are free, and the busy servers track when they will be free. This significantly reduces the complexity by efficiently managing the server states.

### Steps:
1. Use a min-heap to track available servers ordered by weight and index.
2. Use another min-heap for busy servers sorted by the time they become available.
3. For each task, move servers from busy to available if they are free.
4. Assign the task to the best available server, and then add it to the busy servers with the computed finish time.

Here's the code implementation:

```csharp
public int[] AssignTasks(int[] servers, int[] tasks) {
    var availableServers = new SortedSet<(int weight, int index)>();
    var busyServers = new SortedSet<(long endTime, int weight, int index)>();
    int[] result = new int[tasks.Length];
    long currentTime = 0;

    // Initialize available servers
    for (int i = 0; i < servers.Length; i++) {
        availableServers.Add((servers[i], i));
    }

    for (int i = 0; i < tasks.Length; i++) {
        currentTime = Math.Max(currentTime, i);

        // Move finished servers from busy to available
        while (busyServers.Count > 0 && busyServers.Min.endTime <= currentTime) {
            var server = busyServers.Min;
            busyServers.Remove(server);
            availableServers.Add((server.weight, server.index));
        }

        // If no servers are available
        if (availableServers.Count == 0) {
            currentTime = busyServers.Min.endTime;
            while (busyServers.Count > 0 && busyServers.Min.endTime <= currentTime) {
                var server = busyServers.Min;
                busyServers.Remove(server);
                availableServers.Add((server.weight, server.index));
            }
        }

        // Assign the next available server with the least weight and index
        var bestServer = availableServers.Min;
        availableServers.Remove(bestServer);
        result[i] = bestServer.index;
        busyServers.Add((currentTime + tasks[i], bestServer.weight, bestServer.index));
        currentTime++;
    }

    return result;
}
```

### Complexity Analysis:
- **Time Complexity:** O(m * log(n)), where `m` is the number of tasks and `n` is the number of servers. We efficiently handle insertions and deletions into the priority queues using log scale operations.
- **Space Complexity:** O(n), as we store server states referencing both available and busy heap structures.

This solution efficiently leverages priority queues to optimally assign servers to tasks with minimal delay.

