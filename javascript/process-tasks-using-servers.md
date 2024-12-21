# [Leetcode 1882: Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)

## Table of Contents
- [Approach 1: Brute-force solution](#approach-1-brute-force-solution)
- [Approach 2: Priority Queues / Min-Heaps](#approach-2-priority-queues--min-heaps)

---

## Approach 1: Brute-force solution

### Intuition
The brute-force approach involves iterating through the list of servers and tasks and assigns each task to the first available server. This requires checking each server's availability status sequentially, and it does not take advantage of any advanced data structures, potentially resulting in inefficiencies.

### Steps:
1. Initialize a variable to keep track of the current time and an array to store the assignments.
2. Use a loop to iterate over each task in the arrival order.
3. For each task, check every server to find one that is available. This involves iterating through all servers to check their availability before the task is assigned.
4. Assign the task to the first available server and update the server’s availability status.

### Code:
```javascript
function assignTasks(servers, tasks) {
    const n = servers.length;
    const m = tasks.length;
    const result = Array(m).fill(-1);
    const serverAvailability = Array(n).fill(0); // when each server will be available

    for (let t = 0; t < m; t++) { // iterate over each task
        let assigned = false;
        for (let i = 0; i < n; i++) { // iterate over each server
            if (serverAvailability[i] <= t) { // check if the server is available at time t
                result[t] = i; // assign task to this server
                serverAvailability[i] = t + tasks[t]; // update server's next availability time
                assigned = true;
                break;
            }
        }
        if (!assigned) {
            // Increment time until a server becomes available
            let timeIncrement = Math.min(...serverAvailability) - t;
            t += timeIncrement - 1; // -1 because loop will increment t by 1 again
        }
    }
    
    return result;
}
```

### Time Complexity
- The time complexity is O(m * n) where `m` is the number of tasks and `n` is the number of servers. This is because for each task, we might need to check each server.

### Space Complexity
- The space complexity is O(m + n) for storing results and availability times.

---

## Approach 2: Priority Queues / Min-Heaps

### Intuition
To optimize the task assignment, use two priority queues (min-heaps): one for available servers and another for busy servers. We keep both queues sorted based on priority, which allows for efficiently finding the best server for each task and tracking when busy servers become available.

### Steps:
1. Maintain two priority queues: 
   - One for available servers, sorted by weight, and then by index.
   - Another for busy servers, sorted by the time they become available.
2. For each task, first process any servers that have become available by the current task time.
3. If no servers are available, fast forward the time to the nearest time a server becomes available.
4. Assign the current task to the best available server by popping the most suitable server from the available server queue.
5. Push this server onto the busy servers queue with the updated next available time.

### Code:
```javascript
class MinHeap {
    constructor(comparator) {
        this.heap = [];
        this.comparator = comparator ? comparator : (a, b) => a - b;
    }
    
    size() {
        return this.heap.length;
    }
    
    peek() {
        return this.heap[0];
    }
    
    offer(value) {
        this.heap.push(value);
        this._siftUp();
    }
    
    poll() {
        const result = this.peek();
        const last = this.heap.pop();
        if (this.size() > 0) {
            this.heap[0] = last;
            this._siftDown();
        }
        return result;
    }
    
    _siftUp() {
        let nodeIndex = this.size() - 1;
        const node = this.heap[nodeIndex];
        while (nodeIndex > 0) {
            const parentIndex = (nodeIndex - 1) >> 1;
            const parent = this.heap[parentIndex];
            if (this.comparator(node, parent) >= 0) break;
            this.heap[nodeIndex] = parent;
            nodeIndex = parentIndex;
        }
        this.heap[nodeIndex] = node;
    }
    
    _siftDown() {
        let nodeIndex = 0;
        const node = this.heap[nodeIndex];
        const length = this.size();
        while (nodeIndex < length >> 1) {
            let smallestChildIndex = (nodeIndex << 1) + 1;
            const rightChildIndex = smallestChildIndex + 1;
            if (rightChildIndex < length &&
                this.comparator(this.heap[rightChildIndex], this.heap[smallestChildIndex]) < 0) {
                smallestChildIndex = rightChildIndex;
            }
            if (this.comparator(node, this.heap[smallestChildIndex]) <= 0) break;
            this.heap[nodeIndex] = this.heap[smallestChildIndex];
            nodeIndex = smallestChildIndex;
        }
        this.heap[nodeIndex] = node;
    }
}

function assignTasks(servers, tasks) {
    const n = servers.length;
    const availableServers = new MinHeap((a, b) => {
        if (a[0] !== b[0]) return a[0] - b[0]; // compare weight first
        return a[1] - b[1]; // then index
    });

    const busyServers = new MinHeap((a, b) => a[0] - b[0]); // compare available time
    const result = Array(tasks.length);

    // Initialize the available servers heap
    for (let i = 0; i < n; i++) {
        availableServers.offer([servers[i], i]);
    }

    let time = 0;
    for (let t = 0; t < tasks.length; t++) {
        time = Math.max(time, t);
        
        // Move servers that have finished processing tasks back to available
        while (busyServers.size() > 0 && busyServers.peek()[0] <= time) {
            const [availTime, serverWeight, serverIndex] = busyServers.poll();
            availableServers.offer([serverWeight, serverIndex]);
        }
        
        // If no servers are currently available, fast forward time
        if (availableServers.size() === 0) {
            time = busyServers.peek()[0];
            while (busyServers.size() > 0 && busyServers.peek()[0] <= time) {
                const [availTime, serverWeight, serverIndex] = busyServers.poll();
                availableServers.offer([serverWeight, serverIndex]);
            }
        }
        
        // Assign the task to the server
        const [serverWeight, serverIndex] = availableServers.poll();
        result[t] = serverIndex;
        busyServers.offer([time + tasks[t], serverWeight, serverIndex]);
    }

    return result;
}
```

### Time Complexity
- The time complexity is O((m + n) log n) because each task requires operations involving insertion and removal from the min-heaps.

### Space Complexity
- The space complexity is O(n) required for the heaps.

