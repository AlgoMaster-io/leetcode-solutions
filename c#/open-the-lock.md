# [Leetcode 752: Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Solutions
- [Approach 1: Breadth-First Search (BFS) with Queue](#approach-1-bfs-with-queue)

---

### Approach 1: Breadth-First Search (BFS) with Queue

#### Intuition:
The trickiest part of this problem is treating it as a shortest path problem on a graph. Each "lock position" (0000, 0001, etc.) is considered a node, and rotating one wheel up or down is an edge. Your task is to find the shortest path from the start node "0000" to the end node "target", accounting for "deadends".

A Breadth-First Search (BFS) is appropriate because it explores all nodes at the present "depth" (number of moves) before moving on to nodes at the next depth level. This helps find the shortest path efficiently.

- Start from the initial "0000", check all possible combinations by spinning each wheel both forward and backward.
- Use a queue to facilitate the BFS.
- Track visited nodes to avoid cycles and redundant operations.
- A set of deadends is given where any lock combination included in it cannot be visited.

#### Detailed Code Explanation:
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public int OpenLock(string[] deadends, string target) {
        // Initialize deadends as a HashSet for O(1) lookup
        var deadSet = new HashSet<string>(deadends);
        
        // If the initial state "0000" is a deadend, return -1 since we can't start
        if (deadSet.Contains("0000")) return -1;
        
        // The BFS queue, starts with the initial state "0000"
        var queue = new Queue<string>();
        queue.Enqueue("0000");

        // Track visited states to avoid repeating same state
        var visited = new HashSet<string>();
        visited.Add("0000");

        // Start with 0 number of turns
        int turns = 0;
        
        // Directions to move the wheel: +1 and -1
        var directions = new int[] { 1, -1 };
        
        // While there are states to explore
        while (queue.Count > 0) {
            int size = queue.Count;
            
            // Explore all states on the current level
            for (int i = 0; i < size; i++) {
                string current = queue.Dequeue();
                
                // If we reached the target, return the number of turns
                if (current == target) return turns;
                
                // Attempt to spin each wheel in both directions
                for (int j = 0; j < 4; j++) {
                    foreach (int direction in directions) {
                        char[] newLockPosition = current.ToCharArray();
                        newLockPosition[j] = (char)((newLockPosition[j] - '0' + direction + 10) % 10 + '0');
                        string newLock = new string(newLockPosition);
                        
                        // If this new position is neither visited nor a deadend, add it to the queue
                        if (!visited.Contains(newLock) && !deadSet.Contains(newLock)) {
                            visited.Add(newLock);
                            queue.Enqueue(newLock);
                        }
                    }
                }
            }
            // Increment the number of turns after exploring current depth level
            turns++;
        }
        // If we exhaust the queue without finding the target, return -1
        return -1;
    }
}
```

#### **Complexity Analysis:**

- **Time Complexity:** \( O(N^2 \times M^2) \)
  - `N` is the number of digits in the lock, fixed at 4 in this problem.
  - `M` is the possible choices for each digit (10 in this case, 0-9).
  - Each combination can potentially create up to \(10^N\) states to examine.
  - BFS ensures we do not revisit already visited nodes, hence limiting repeated work.

- **Space Complexity:** \( O(M^N) \)
  - The worst-case size of the queue is the number of nodes, which is at most \(10^4\) (10000 possible lock combinations).
  - Additional space for recording visited nodes and deadends.

By employing BFS, this solution efficiently finds the shortest path while circumventing deadends and unreachable states.

