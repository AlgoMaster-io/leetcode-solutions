## [Leetcode 621: Task Scheduler](https://leetcode.com/problems/task-scheduler/)

### Approaches:
1. [Greedy Scheduling with Priority Queue](#approach-1)
2. [Greedy Scheduling with Frequency Calculations](#approach-2)

---

### Approach 1: Greedy Scheduling with Priority Queue

**Intuition:**

The problem can be visualized as filling slots in a schedule but ensuring that there are enough cooldown periods between the same tasks. The most basic approach involves using a priority queue to always schedule the task that is left with the highest frequency. This strategy simulates the task execution by tracking time and rearranging tasks based on cooldowns.

1. **Frequency Counting:** First, calculate the frequency of each task.
2. **Priority Queue:** Use a max heap (priority queue) to always schedule the most frequent task available, as this task contributes the most to minimizing the idle intervals.
3. **Cooldown Management:** Use a cooldown queue to track tasks that are in cooldown periods and need to be paused before they can be scheduled again.
4. **Time Simulation:** Iterate over time units and use steps 2 and 3 above to decide what task, if any, to execute at each time step.

**Time Complexity:** O(N log k), where N is the number of tasks and k is the number of unique tasks.

**Space Complexity:** O(N), as we are storing the frequencies and cooldown tasks.

```java
import java.util.*;

public class TaskScheduler {

    public int leastInterval(char[] tasks, int n) {
        Map<Character, Integer> frequencyMap = new HashMap<>();
        
        // Step 1: Count the frequency of each task
        for (char task : tasks) {
            frequencyMap.put(task, frequencyMap.getOrDefault(task, 0) + 1);
        }

        // Step 2: Initialize a max heap based on task frequencies
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        maxHeap.addAll(frequencyMap.values());

        // This map will track tasks in their cooldown period
        Queue<Map.Entry<Integer, Integer>> cooldownQueue = new LinkedList<>();
        
        int time = 0;

        // Step 3: Simulate the time units
        while (!maxHeap.isEmpty() || !cooldownQueue.isEmpty()) {
            time++;
            
            if (!maxHeap.isEmpty()) {
                int currentTaskFreq = maxHeap.poll() - 1; // Execute task with max frequency
                
                if (currentTaskFreq > 0) {
                    // Task needs to cool down before re-invoking
                    cooldownQueue.offer(new AbstractMap.SimpleEntry<>(currentTaskFreq, time + n));
                }
            }

            // Step 4: Check cooldown queue if any task is ready to re-enter the max heap
            if (!cooldownQueue.isEmpty() && cooldownQueue.peek().getValue() == time) {
                maxHeap.offer(cooldownQueue.poll().getKey());
            }
        }

        return time;
    }
}
```

---

### Approach 2: Greedy Scheduling with Frequency Calculations

**Intuition:**

The most optimal solution to this problem is to directly calculate the least interval based on the required cooling periods and the frequencies of the most often appearing tasks. We can use a mathematical approach to determine how to place the tasks efficiently.

1. **Find Maximum Frequency:** Determine the maximum frequency tasks need to be repeated.
2. **Slots Calculation:** Calculate the possible slots needed to place the most frequent tasks and any additional tasks that fit in the gaps.
3. **Calculate Idle Units:** Calculate exactly how many idle units are needed after placing other tasks between the maximum frequency tasks.
4. **Total Time Calculation:** Determine the total required time by summing filled slots and idle time.

**Time Complexity:** O(N), where N is the number of tasks.

**Space Complexity:** O(1), only constant extra space is utilized (e.g. for counting frequencies).

```java
import java.util.*;

public class TaskScheduler {

    public int leastInterval(char[] tasks, int n) {
        int[] frequencies = new int[26];

        // Step 1: Count the frequency of each task
        for (char task : tasks) {
            frequencies[task - 'A']++;
        }

        // Step 2: Find the maximum frequency
        Arrays.sort(frequencies);
        int maxFreq = frequencies[25];
        int maxCount = 0;
        
        for (int freq : frequencies) {
            if (freq == maxFreq) {
                maxCount++;
            }
        }

        // Step 3: Calculate the total intervals
        int partCount = maxFreq - 1;
        int partLength = n - (maxCount - 1);
        int emptySlots = partCount * partLength;
        int availableTasks = tasks.length - maxFreq * maxCount;
        int idles = Math.max(0, emptySlots - availableTasks);

        return tasks.length + idles;
    }
}
```

This mathematical approach leverages slot filling with the most frequent tasks first to naturally minimize idle time and achieves optimal scheduling.

