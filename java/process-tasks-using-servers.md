# [Leetcode 1882: Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)

## Approaches
- [Approach 1: Brute-force Simulation](#approach-1-brute-force-simulation)
- [Approach 2: Priority Queues for Efficient Assignment](#approach-2-priority-queues-for-efficient-assignment)

## Approach 1: Brute-force Simulation

### Intuition
In a brute-force simulation approach, we can use an array to keep track of each server's availability. For each task, we iterate through the list of servers to find an available one. If none are available, we hold off the task until a server becomes free. This direct simulation is simple but not efficient because checking each server for availability takes linear time.

### Steps
1. Use an array to represent available servers. Initialize it all to zero, representing that all servers are initially free.
2. Iterate over each task:
   - Check all servers to find an available one. Mark the server as busy for the duration of the task.
   - If no server is free, wait until the earliest available server becomes free.
3. Summarize the task allocation to servers.

### Time Complexity
- **O(n * k)** where n is the number of tasks and k is the number of servers.

### Space Complexity
- **O(k)** for keeping track of the state of each server.

```java
public int[] assignTasks(int[] servers, int[] tasks) {
    int numServers = servers.length;
    int numTasks = tasks.length;
    
    // Save the completion time of servers
    int[] serverEnds = new int[numServers];
    int[] result = new int[numTasks];
    
    for (int i = 0; i < numTasks; i++) {
        int task = tasks[i];
        int availableServerIndex = -1;
        
        // Find the earliest available server
        for (int j = 0; j < numServers; j++) {
            if (i >= serverEnds[j]) {
                if (availableServerIndex == -1 || servers[j] < servers[availableServerIndex] ||
                    (servers[j] == servers[availableServerIndex] && j < availableServerIndex)) {
                    availableServerIndex = j;
                }
            }
        }
        
        // No server is available, need to wait
        if (availableServerIndex == -1) {
            int earliestServerEndTime = Integer.MAX_VALUE;
            for (int j = 0; j < numServers; j++) {
                if (serverEnds[j] < earliestServerEndTime) {
                    earliestServerEndTime = serverEnds[j];
                    availableServerIndex = j;
                }
            }
        }
        
        // Assign the task to the selected server
        result[i] = availableServerIndex;
        serverEnds[availableServerIndex] = Math.max(serverEnds[availableServerIndex], i) + task;
    }
    
    return result;
}
```

## Approach 2: Priority Queues for Efficient Assignment

### Intuition
The brute-force approach makes a key observation: it often checks all servers unnecessarily. We can optimize this using two priority queues:
- One for free servers, sorted by their weight and index.
- Another for busy servers, including when they become available again.

With these queues, tasks can be assigned more efficiently by popping from the priority queue.

### Steps
1. Create two priority queues: one for available servers and one for busy servers.
2. Iterate over tasks, and for each task, update the queue of busy servers to free any that have completed their tasks.
3. Check the available servers queue. Assign the task to the server with the smallest weight and index.
4. If no servers are free, update the next server that becomes available by waiting.

### Time Complexity
- **O((n + k) * log k)**, as each task may involve operations in the priority queue.

### Space Complexity
- **O(k)**, for maintaining server state.

```java
import java.util.PriorityQueue;
import java.util.Comparator;

public int[] assignTasks(int[] servers, int[] tasks) {
    int n = servers.length;
    int m = tasks.length;
    int[] result = new int[m];

    // PriorityQueue for available and busy servers
    PriorityQueue<int[]> available = new PriorityQueue<>(Comparator.comparingInt((int[] a) -> a[0]).thenComparingInt(a -> a[1]));
    PriorityQueue<int[]> busy = new PriorityQueue<>(Comparator.comparingInt((int[] a) -> a[0]));

    for (int i = 0; i < n; i++) {
        available.offer(new int[] {servers[i], i});
    }

    int time = 0;
    for (int i = 0; i < m; i++) {
        time = Math.max(time, i);

        // Release servers that are no longer busy
        while (!busy.isEmpty() && busy.peek()[0] <= time) {
            int[] server = busy.poll();
            available.offer(new int[] {servers[server[1]], server[1]});
        }

        // If no servers are available, jump the time to the next server free time
        if (available.isEmpty()) {
            time = busy.peek()[0];
            while (!busy.isEmpty() && busy.peek()[0] <= time) {
                int[] server = busy.poll();
                available.offer(new int[] {servers[server[1]], server[1]});
            }
        }

        int[] server = available.poll();
        result[i] = server[1];
        busy.offer(new int[] {time + tasks[i], server[1]});
    }

    return result;
}
```

These approaches provide different balances of simplicity and efficiency for task-server allocation problems, and choosing the right one depends on constraints and desired outcomes.

