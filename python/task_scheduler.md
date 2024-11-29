# 621. [Task Scheduler](https://leetcode.com/problems/task-scheduler/)

## Approach: Greedy Algorithm with Counting

### Solution
```python
# Time Complexity: O(n), where n is the total number of tasks
# Space Complexity: O(1), as the task frequencies are stored in a fixed-size array

class Solution:
    def leastInterval(self, tasks, n):
        freq = [0] * 26  # Array to store task frequencies

        # Count the frequency of each task
        for task in tasks:
            freq[ord(task) - ord('A')] += 1

        # Sort the frequencies in descending order
        freq.sort()

        maxFreq = freq[25]  # Highest frequency
        idleSlots = (maxFreq - 1) * n  # Calculate idle slots

        # Deduct the available slots with other task frequencies
        for i in range(24, -1, -1):
            if freq[i] > 0:
                idleSlots -= min(freq[i], maxFreq - 1)

        # If idleSlots are negative, it means no idling is needed
        idleSlots = max(0, idleSlots)

        # Total time is tasks + idle slots
        return len(tasks) + idleSlots
```

