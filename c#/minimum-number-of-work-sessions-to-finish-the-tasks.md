# [Leetcode 1986: Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approaches
- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Dynamic Programming (Bitmask)](#approach-2-dynamic-programming-bitmask)

### Approach 1: Recursive Backtracking

The idea is to use backtracking to try out all possible ways to assign tasks into different work sessions. We keep track of the current session time and decide whether to put each task into the current session or open a new session.

#### Intuition
- Start from the first task and try adding it to the current session.
- If the addition exceeds the session time limit, start a new session.
- Repeat the process for all tasks while keeping track of the minimum number of sessions needed.

```csharp
public class Solution {
    private int minSessions = int.MaxValue;

    public int MinSessions(int[] tasks, int sessionTime) {
        Array.Sort(tasks);
        Array.Reverse(tasks);

        Backtrack(tasks, new List<int>(), sessionTime, 0);
        
        return minSessions;
    }

    private void Backtrack(int[] tasks, List<int> sessions, int sessionTime, int index) {
        if (index == tasks.Length) {
            int sessionCount = sessions.Count;
            if (sessions[^1] == 0) sessionCount--;
            minSessions = Math.Min(minSessions, sessionCount);
            return;
        }

        for (int i = 0; i < sessions.Count; i++) {
            if (sessions[i] + tasks[index] <= sessionTime) {
                sessions[i] += tasks[index];
                Backtrack(tasks, sessions, sessionTime, index + 1);
                sessions[i] -= tasks[index];
            }
        }
        
        sessions.Add(tasks[index]);
        Backtrack(tasks, sessions, sessionTime, index + 1);
        sessions.RemoveAt(sessions.Count - 1);
    }
}
```

**Complexity Analysis:**
- **Time Complexity:** O(n!), where n is the number of tasks. This is due to the exploration of all permutations.
- **Space Complexity:** O(n), used for the recursive call stack and tracking sessions.

---

### Approach 2: Dynamic Programming (Bitmask)

In this approach, we use bitmask DP to keep track of which tasks have been completed. We try to minimize the number of sessions used by checking all possible subsets of tasks that can fit into the session time.

#### Intuition
- Use a bitmask to represent subsets of tasks that we have completed.
- For each mask, calculate the minimum number of sessions needed.
- For each bitmask, check its subsets to see if they fit within a session.
- Update our dp array with the minimum sessions required for each mask.

```csharp
public class Solution {
    public int MinSessions(int[] tasks, int sessionTime) {
        int n = tasks.Length;
        int total = 1 << n;

        int[] minSessions = new int[total];
        Array.Fill(minSessions, n + 1);
        minSessions[0] = 0;

        for (int mask = 0; mask < total; mask++) {
            int usedTime = sessionTime + 1;
            for (int i = 0; i < n; i++) {
                if ((mask & (1 << i)) != 0) {
                    usedTime -= tasks[i];
                }
            }
            
            for (int subsetMask = mask; subsetMask > 0; subsetMask = (subsetMask - 1) & mask) {
                int subsetUsedTime = 0;
                for (int i = 0; i < n; i++) {
                    if ((subsetMask & (1 << i)) != 0) {
                        subsetUsedTime += tasks[i];
                    }
                }
                
                if (subsetUsedTime <= sessionTime) {
                    minSessions[mask] = Math.Min(minSessions[mask], 1 + minSessions[mask ^ subsetMask]);
                }
            }
        }

        return minSessions[total - 1];
    }
}
```

**Complexity Analysis:**
- **Time Complexity:** O(n * 2^n), where n is the number of tasks. Iterate through all subsets of tasks represented by bitmasks.
- **Space Complexity:** O(2^n), required to store the number of sessions needed for each bitmask.

In conclusion, we have explored two different approaches ranging from recursive backtracking to a more optimal dynamic programming approach using bitmasking for the "Minimum Number of Work Sessions to Finish the Tasks" problem. The recursive backtracking provides a direct, albeit less efficient, means of solving the problem by exploring permutations. The dynamic programming approach improves efficiency, especially for larger numbers of tasks.

