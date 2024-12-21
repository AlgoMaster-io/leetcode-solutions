# Leetcode Problem: [621. Task Scheduler](https://leetcode.com/problems/task-scheduler/)

## Solutions
- [Approach 1: Simulate the Execution using Priority Queue](#approach-1-simulate-the-execution-using-priority-queue)
- [Approach 2: Mathematics with Count Array](#approach-2-mathematics-with-count-array)

### Approach 1: Simulate the Execution using Priority Queue

**Intuition:**

This approach simulates executing tasks while respecting the idle time constraints. We will use a max-heap (priority queue) to always execute the task with the highest frequency first, ensuring the tasks are distributed as evenly as possible across the time slots.

**Steps:**
1. Count the frequency of each task and use a max heap to store them.
2. While there are tasks left in the heap, try to schedule them in chunks defined by the cooling period.
3. If the heap still has tasks and we haven't filled the cooldown slots, we'll schedule ‘idle’ times to satisfy the constraints.
4. Return the total time taken which includes both tasks and idle time.

```typescript
function leastInterval(tasks: string[], n: number): number {
    const taskMap = new Map<string, number>();

    // Count the occurrences of each task
    tasks.forEach(task => {
        taskMap.set(task, (taskMap.get(task) || 0) + 1);
    });

    // Create a max heap of the tasks based on their frequency
    const maxHeap: number[] = [...taskMap.values()].sort((a, b) => b - a);

    let time = 0;

    while (maxHeap.length) {
        let i = 0;
        const temp: number[] = [];

        // Process up to n+1 tasks
        while (i <= n) {
            if (maxHeap.length) {
                if (maxHeap[0] > 1) {
                    temp.push(maxHeap[0] - 1);
                }
                maxHeap.shift();
            }

            time++;
            if (!maxHeap.length && !temp.length) break;
            i++;
        }

        // Push back the non-zero frequency tasks into the max heap
        maxHeap.push(...temp);
        maxHeap.sort((a, b) => b - a);
    }

    return time;
}
```

**Complexity:**
- Time Complexity: O(T * N), where T is the number of tasks, and N is the cooling period.
- Space Complexity: O(1), though we use a fixed heap size.

### Approach 2: Mathematics with Count Array

**Intuition:**

This approach calculates how to optimally distribute tasks based on their frequency to reduce the number of idle times required. We focus on the task with the highest frequency to plan our schedule.

**Steps:**
1. Count the maximum frequency of any task and how many tasks have this frequency.
2. Calculate the idle slots needed based on the most frequent task.
3. Subtract from the idle slots the number of tasks that can be scheduled in these slots, calculated by avoiding over-counting the most frequent task kind.
4. If there are still idle slots left after placing tasks, they become real idle times.

```typescript
function leastInterval(tasks: string[], n: number): number {
    const freq = new Array(26).fill(0);
    
    // Count the frequency of each task
    for (let task of tasks) {
        freq[task.charCodeAt(0) - 'A'.charCodeAt(0)]++;
    }

    // Sort frequencies to process the most frequent tasks first
    freq.sort((a, b) => b - a);

    const maxFreq = freq[0];
    let idleTime = (maxFreq - 1) * n;

    // Reduce idle period using remaining tasks
    for (let i = 1; i < freq.length; i++) {
        if (freq[i] > 0) {
            idleTime -= Math.min(maxFreq - 1, freq[i]);
        }
    }

    // Ensure no negative idle time
    idleTime = Math.max(0, idleTime);

    return tasks.length + idleTime;
}
```

**Complexity:**
- Time Complexity: O(T + N), where T is the number of tasks, and N is the cooling period.
- Space Complexity: O(1), for the fixed size of the frequency count array.

These approaches showcase different strategies to address task scheduling with cooling periods in a computational and mathematical manner.

