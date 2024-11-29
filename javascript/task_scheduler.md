# 621. [Task Scheduler](https://leetcode.com/problems/task-scheduler/)

## Approach: Greedy Algorithm with Counting

### Solution

```javascript
// Time Complexity: O(n), where n is the total number of tasks
// Space Complexity: O(1), as the task frequencies are stored in a fixed-size array

var leastInterval = function(tasks, n) {
    const freq = new Array(26).fill(0); // Array to store task frequencies

    // Count the frequency of each task
    for (let task of tasks) {
        freq[task.charCodeAt(0) - 'A'.charCodeAt(0)]++;
    }

    // Sort the frequencies in descending order
    freq.sort((a, b) => a - b);

    const maxFreq = freq[25]; // Highest frequency
    let idleSlots = (maxFreq - 1) * n; // Calculate idle slots

    // Deduct the available slots with other task frequencies
    for (let i = 24; i >= 0 && freq[i] > 0; i--) {
        idleSlots -= Math.min(freq[i], maxFreq - 1);
    }

    // If idleSlots are negative, it means no idling is needed
    idleSlots = Math.max(0, idleSlots);

    // Total time is tasks + idle slots
    return tasks.length + idleSlots;
};
```

