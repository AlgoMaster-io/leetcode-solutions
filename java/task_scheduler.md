# 621. [Task Scheduler](https://leetcode.com/problems/task-scheduler/)

## Approach: Greedy Algorithm with Counting

### Solution
```java
// Time Complexity: O(n), where n is the total number of tasks
// Space Complexity: O(1), as the task frequencies are stored in a fixed-size array
import java.util.Arrays;

public class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] freq = new int[26]; // Array to store task frequencies

        // Count the frequency of each task
        for (char task : tasks) {
            freq[task - 'A']++;
        }

        // Sort the frequencies in descending order
        Arrays.sort(freq);

        int maxFreq = freq[25]; // Highest frequency
        int idleSlots = (maxFreq - 1) * n; // Calculate idle slots

        // Deduct the available slots with other task frequencies
        for (int i = 24; i >= 0 && freq[i] > 0; i--) {
            idleSlots -= Math.min(freq[i], maxFreq - 1);
        }

        // If idleSlots are negative, it means no idling is needed
        idleSlots = Math.max(0, idleSlots);

        // Total time is tasks + idle slots
        return tasks.length + idleSlots;
    }
}
```