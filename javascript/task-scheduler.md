## [Leetcode 621: Task Scheduler](https://leetcode.com/problems/task-scheduler/)

### Approaches:
1. [Brute Force](#brute-force)
2. [Greedy with Priority Queue](#greedy-with-priority-queue)
3. [Optimal Math Approach](#optimal-math-approach)

### Brute Force

#### Intuition:
In the brute force approach, the idea is to simulate the process of executing tasks using a cooldown period. We maintain a running list to track the order of task execution and ensure no task is scheduled before its cooling period (n). This involves repeatedly checking if a task can be executed in the current slot, selecting other tasks if needed.

#### Code:

```javascript
function leastInterval(tasks, n) {
    // Calculate the frequency of each task.
    const frequencyMap = new Map();
    for (const task of tasks) {
        frequencyMap.set(task, (frequencyMap.get(task) || 0) + 1);
    }

    // Create a list of task frequencies.
    const frequencies = [...frequencyMap.values()];

    // Sort frequencies in descending order to consider the most frequent tasks first.
    frequencies.sort((a, b) => b - a);

    let time = 0;

    while (frequencies[0] > 0) {
        let i = 0;
        // Loop up to n+1 to simulate the task run period
        while (i <= n) {
            if (frequencies[0] === 0) {
                break; // If the most frequent task has no more instances left, break.
            }
            if (i < frequencies.length && frequencies[i] > 0) {
                frequencies[i]--; // Decrease the frequency of the current task.
            }
            time++; // Increase the total time.
            i++;
        }
        // Re-sort the frequencies after scheduling to handle the remaining tasks in next cycles.
        frequencies.sort((a, b) => b - a);
    }

    return time;
}
```

#### Complexity:
- **Time Complexity:** O(T * n), where T is the total number of tasks. Each time we perform a full n-cycle at most O(T) times, for each valid task frequency.
- **Space Complexity:** O(1) aside from the input data, as a fixed number of operations are performed.

### Greedy with Priority Queue

#### Intuition:
In this approach, we use a max heap (priority queue) to always process the most frequent task available. We maintain a separate list for tracking cooldowns. In each cycle, we take the highest frequency task from the heap, execute it, and decrease its frequency. If it still has pending instances, we push it to the cooldown system.

#### Code:

```javascript
function leastInterval(tasks, n) {
    const frequencyMap = new Map();
    for (const task of tasks) {
        frequencyMap.set(task, (frequencyMap.get(task) || 0) + 1);
    }

    const maxHeap = new MaxPriorityQueue({
        priority: (task) => task[1]
    });

    frequencyMap.forEach((freq, task) => {
        maxHeap.enqueue([task, freq]);
    });

    let time = 0;

    while (!maxHeap.isEmpty()) {
        const waitList = [];
        let cycles = n + 1; // We can work on at most n+1 different tasks in each cycle

        while (cycles > 0 && !maxHeap.isEmpty()) {
            const [task, freq] = maxHeap.dequeue().element;
            if (freq > 1) {
                waitList.push([task, freq - 1]); // Decrease frequency by 1 and push to waitList
            }
            time++;
            cycles--;
        }

        waitList.forEach((item) => maxHeap.enqueue(item)); // Re-insert tasks with updated frequencies

        if (maxHeap.isEmpty()) {
            break;
        }

        time += cycles; // Basically, add idle time if cycle was not processed fully
    }

    return time;
}
```

#### Complexity:
- **Time Complexity:** O(T log T), operations for max heap insertion/removal are logarithmic, repeated for each task.
- **Space Complexity:** O(T), space for storing tasks in heap.

### Optimal Math Approach

#### Intuition:
The optimal approach leverages mathematical reasoning about the task cycle required to minimize idle time. The idea focuses on arranging the most frequent tasks in slots that respect the cooldown n. The algorithm calculates the number of full "frames" needed by the most frequent tasks, then determines if extra slots (created by the less frequent tasks and configurations) can fill these gaps.

#### Code:

```javascript
function leastInterval(tasks, n) {
    const frequencyMap = new Map();
    for (const task of tasks) {
        frequencyMap.set(task, (frequencyMap.get(task) || 0) + 1);
    }

    const frequencies = [...frequencyMap.values()];
    const maxFrequency = Math.max(...frequencies);
    let maxCount = 0;
    
    // Count how many tasks have the maximum frequency
    frequencies.forEach(freq => {
        if (freq === maxFrequency) maxCount++;
    });

    // (maxFrequency - 1) * (n + 1) creates the necessary frame for the most frequent task
    // and maxCount fills the last row of the frame
    const partCount = maxFrequency - 1;
    const emptySlots = partCount * (n - (maxCount - 1));
    const availableTasks = tasks.length - maxFrequency * maxCount;
    const idles = Math.max(0, emptySlots - availableTasks);

    return tasks.length + idles;
}
```

#### Complexity:
- **Time Complexity:** O(T), where T is the number of tasks, for counting frequencies and making calculations.
- **Space Complexity:** O(1), does not use additional space proportionate to input size, just uses fixed number of additional variables and utility storage.

