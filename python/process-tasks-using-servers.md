# [Leetcode 1882: Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Min-Heap (Efficient)](#approach-2-min-heap-efficient)

---

## Approach 1: Brute Force

### Intuition

The brute force approach involves iterating over each server and each task, checking availability after each task is completed. This approach checks each server at every task's start time to see if it is available to process the task.

### Solution

```python
def assignTasks(servers, tasks):
    server_count = len(servers)
    task_count = len(tasks)
    result = [-1] * task_count
    
    # Initialize server times to 0 (available from the start)
    server_times = [0] * server_count
    
    # Iterate over each task
    for start_time in range(task_count):
        task_done = False
        for i in range(server_count):
            # Check if server is available
            if server_times[i] <= start_time:
                result[start_time] = i
                server_times[i] = start_time + tasks[start_time]
                task_done = True
                break
        if not task_done:
            min_time = float('inf')
            chosen_server = -1
            for i in range(server_count):
                if server_times[i] < min_time:
                    min_time = server_times[i]
                    chosen_server = i
            result[start_time] = chosen_server
            server_times[chosen_server] = server_times[chosen_server] + tasks[start_time]
            
    return result
```

### Complexity Analysis

- **Time Complexity:** O(n * m), where `n` is the number of tasks and `m` is the number of servers. This is due to checking server availability for each task.
- **Space Complexity:** O(n), to store the result of server assignments.

---

## Approach 2: Min-Heap (Efficient)

### Intuition

We can make the solution more efficient by using min-heaps. By maintaining two heaps - one for the free servers sorted by time and index, and another for busy servers by the time they become free - we can efficiently assign tasks as soon as they're available, minimizing idle periods.

### Solution

```python
import heapq

def assignTasks(servers, tasks):
    server_count = len(servers)
    # Min-heap for free servers
    free_servers = [(weight, index) for index, weight in enumerate(servers)]
    heapq.heapify(free_servers)
    # Min-heap for busy servers
    busy_servers = []
    result = []
    time = 0
    
    for t, task_time in enumerate(tasks):
        time = max(time, t)
        
        # Free up all servers whose task has ended by current time
        while busy_servers and busy_servers[0][0] <= time:
            end_time, weight, index = heapq.heappop(busy_servers)
            heapq.heappush(free_servers, (weight, index))
        
        # If no server is free, move forward to the next server free time
        if not free_servers:
            time = busy_servers[0][0]
            while busy_servers and busy_servers[0][0] <= time:
                end_time, weight, index = heapq.heappop(busy_servers)
                heapq.heappush(free_servers, (weight, index))
        
        weight, index = heapq.heappop(free_servers)
        result.append(index)
        heapq.heappush(busy_servers, (time + task_time, weight, index))
    
    return result
```

### Complexity Analysis

- **Time Complexity:** O((n + m) log m), where `n` is the number of tasks and `m` is the number of servers. This is mainly due to heap operations.
- **Space Complexity:** O(m), as we store all servers in heaps and result list.

