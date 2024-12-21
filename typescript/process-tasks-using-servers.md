## [Leetcode 1882: Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)

### Table of Contents
1. [Solution 1: Brute Force](#solution-1)
2. [Solution 2: Priority Queue with Two Heaps](#solution-2)

---

### Solution 1: Brute Force

Intuition:
In the brute force approach, we monitor each server to see if it's available at the start of each task's processing time. We iterate through the list of servers and select the available one with the smallest weight. If no servers are available, we wait for the next available server to become free.

#### Code:

```typescript
function assignTasks(servers: number[], tasks: number[]): number[] {
    // Result array to hold the assigned server indices for each task
    const result: number[] = new Array(tasks.length);
    // Keeps track of when each server will be free
    const freeTimes: number[] = new Array(servers.length).fill(0);

    for (let i = 0; i < tasks.length; i++) {
        let serverIndex = -1;
        let currentShortestTime = Number.MAX_SAFE_INTEGER;

        for (let j = 0; j < servers.length; j++) {
            // Check if the server is free at current time, i
            if (freeTimes[j] <= i) {
                // Compare servers by weight and then by index
                if (servers[j] < currentShortestTime || 
                   (servers[j] === currentShortestTime && (serverIndex === -1 || j < serverIndex))) {
                    serverIndex = j;
                    currentShortestTime = servers[j];
                }
            }
        }

        if (serverIndex === -1) {
            // If no server is free, wait for the earliest free server
            let earliestTime = Number.MAX_SAFE_INTEGER;
            for (let j = 0; j < servers.length; j++) {
                if (freeTimes[j] < earliestTime || 
                   (freeTimes[j] === earliestTime && (serverIndex === -1 || j < serverIndex))) {
                    serverIndex = j;
                    earliestTime = freeTimes[j];
                }
            }
            i = freeTimes[serverIndex] - 1;  // Move the pointer to the free time
        } 

        result[i] = serverIndex;
        freeTimes[serverIndex] = i + tasks[i];  // Update the free time of used server
    }
    return result;
}
```

#### Complexity:
- **Time Complexity**: O(m * n), where m is the number of tasks and n is the number of servers. In the worst case, each task has to check all servers.
- **Space Complexity**: O(m), where m is the length of the freeTimes array used to track when each server will become available.

---

### Solution 2: Priority Queue with Two Heaps

Intuition:
To improve efficiency, we use two priority queues (min-heaps):
1. One for the available servers, which sorts by `(weight, index)`.
2. Another for the busy servers, which sorts by `(free time, weight, index)`.

As each task arrives, we process the servers that become free before the current task time. We then either assign a task to the available server with the smallest weight or wait for the next server to become free.

#### Code:

```typescript
class Heap<T extends Array<any>> {
    private data: T[] = [];
    constructor(private compare: (a: T, b: T) => number) {}

    push(item: T) {
        this.data.push(item);
        this.bubbleUp();
    }

    pop(): T | undefined {
        [this.data[0], this.data[this.data.length - 1]] = [this.data[this.data.length - 1], this.data[0]];
        const item = this.data.pop();
        this.bubbleDown();
        return item;
    }

    peek(): T | undefined {
        return this.data[0];
    }

    size() {
        return this.data.length;
    }

    private bubbleUp() {
        let index = this.data.length - 1;
        while (index > 0) {
            let parentIndex = Math.floor((index - 1) / 2);
            if (this.compare(this.data[index], this.data[parentIndex]) >= 0) break;
            [this.data[index], this.data[parentIndex]] = [this.data[parentIndex], this.data[index]];
            index = parentIndex;
        }
    }

    private bubbleDown() {
        let index = 0;
        const lastIndex = this.data.length - 1;
        while (true) {
            let leftChildIdx = 2 * index + 1;
            let rightChildIdx = 2 * index + 2;
            let candidateIdx = index;

            if (leftChildIdx <= lastIndex && this.compare(this.data[leftChildIdx], this.data[candidateIdx]) < 0) {
                candidateIdx = leftChildIdx;
            }

            if (rightChildIdx <= lastIndex && this.compare(this.data[rightChildIdx], this.data[candidateIdx]) < 0) {
                candidateIdx = rightChildIdx;
            }

            if (candidateIdx === index) break;

            [this.data[index], this.data[candidateIdx]] = [this.data[candidateIdx], this.data[index]];
            index = candidateIdx;
        }
    }
}

function assignTasks(servers: number[], tasks: number[]): number[] {
    const available = new Heap<[number, number]>((a, b) => a[0] - b[0] || a[1] - b[1]);
    const busy = new Heap<[number, number, number]>((a, b) => a[0] - b[0] || a[1] - b[1] || a[2] - b[2]);

    for (let i = 0; i < servers.length; i++) {
        available.push([servers[i], i]);
    }

    const result: number[] = [];
    let currentTime = 0;
    
    for (let i = 0; i < tasks.length; i++) {
        currentTime = Math.max(currentTime, i);

        // Free up servers
        while (busy.size() > 0 && busy.peek()![0] <= currentTime) {
            const [freeTime, weight, index] = busy.pop()!;
            available.push([weight, index]);
        }

        if (available.size() === 0) {
            currentTime = busy.peek()![0];
            while (busy.size() > 0 && busy.peek()![0] <= currentTime) {
                const [freeTime, weight, index] = busy.pop()!;
                available.push([weight, index]);
            }
        }
        
        // Assign the task
        const [weight, index] = available.pop()!;
        result.push(index);
        busy.push([currentTime + tasks[i], weight, index]);
    }

    return result;
}
```

#### Complexity:
- **Time Complexity**: O((m + n) * log(n)), where m is the number of tasks and n is the number of servers, due to the operations on the heaps.
- **Space Complexity**: O(n), as we maintain two heaps that can grow in size up to the number of servers and tasks.

In this approach, using priority queues allows more efficient processing compared to iterating through all servers each time, as done in the brute force approach.

