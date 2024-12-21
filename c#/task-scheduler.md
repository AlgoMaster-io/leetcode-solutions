# [Leetcode 621: Task Scheduler](https://leetcode.com/problems/task-scheduler/)

## Approaches:
1. [Basic Approach: Simulation using Priority Queue](#basic-approach-simulation-using-priority-queue)
2. [Optimal Approach: Math + Counting](#optimal-approach-math-counting)

---

### Basic Approach: Simulation using Priority Queue

**Intuition**:
We can simulate the scheduling process using a Priority Queue (Max Heap). The idea is to always execute the task with the highest frequency that’s available to maintain minimum idle time. A cooldown dictionary or array records when a task can be executed again.

```csharp
using System.Collections.Generic;

public class Solution {
    public int LeastInterval(char[] tasks, int n) {
        // Step 1: Count frequency of each task
        Dictionary<char, int> frequency = new Dictionary<char, int>();
        foreach (var task in tasks) {
            if (!frequency.ContainsKey(task)) {
                frequency[task] = 0;
            }
            frequency[task]++;
        }

        // Step 2: MaxHeap to always pick the most frequent task available
        PriorityQueue<int, int> maxHeap = new PriorityQueue<int, int>(Comparer<int>.Create((a, b) => b - a));
        foreach (var freq in frequency.Values) {
            maxHeap.Enqueue(freq, freq);
        }

        int time = 0;
        Queue<int> cooldown = new Queue<int>();

        // Step 3: Simulate every unit of time
        while (maxHeap.Count > 0 || cooldown.Count > 0) {
            time++;

            // Step 4: Schedule task with highest frequency
            if (maxHeap.Count > 0) {
                int currentFreq = maxHeap.Dequeue();
                if (--currentFreq > 0) {
                    cooldown.Enqueue(currentFreq);
                }
            }

            // Step 5: Move tasks from cooldown back to maxHeap
            if (cooldown.Count > n) {
                maxHeap.Enqueue(cooldown.Dequeue(), cooldown.Peek());
            }
        }

        return time;
    }
}
```

**Time Complexity**: \(O(T \log T)\), where \(T\) is the number of unique tasks because each enqueue and dequeue operation on the heap requires logarithmic time.
  
**Space Complexity**: \(O(T)\), since we store the frequency of each task.

### Optimal Approach: Math + Counting

**Intuition**:
The idea is to think of the problem in terms of slots for the most frequent tasks and filling the necessary idle slots between those tasks. The formula relates to the most frequent task and computes the needed idle slots around those tasks.

```csharp
using System;

public class Solution {
    public int LeastInterval(char[] tasks, int n) {
        // Step 1: Count frequency of each task
        int[] frequency = new int[26];
        foreach (var task in tasks) {
            frequency[task - 'A']++;
        }

        // Step 2: Find maximum frequency
        Array.Sort(frequency);
        int fMax = frequency[25];

        // Step 3: Calculate initial idle slots
        int idleTime = (fMax - 1) * n;

        // Step 4: Reduce idle slots by filling with remaining tasks
        for (int i = 24; i >= 0 && idleTime > 0; i--) {
            idleTime -= Math.Min(fMax - 1, frequency[i]);
        }

        // Step 5: Calculate result - remaining idle slots cannot be less than 0
        return Math.Max(tasks.Length, tasks.Length + idleTime);
    }
}
```

**Time Complexity**: \(O(N + K \log K)\), where \(N\) is the number of tasks and \(K\) is the number of unique types of tasks.

**Space Complexity**: \(O(1)\), constant space as the frequency array is of fixed size 26.

