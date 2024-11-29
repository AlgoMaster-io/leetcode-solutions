# 752. [Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Approach: Breadth-First Search (BFS)

### Solution
csharp
```csharp
// Time Complexity: O(10^4 + d), where d is the size of the deadends
// Space Complexity: O(10^4 + d), for the visited set and the queue
using System;
using System.Collections.Generic;

public class Solution {
    public int OpenLock(string[] deadends, string target) {
        HashSet<string> deadSet = new HashSet<string>();
        foreach (string dead in deadends) {
            deadSet.Add(dead);
        }

        if (deadSet.Contains("0000")) {
            return -1; // Cannot start from the initial position
        }

        Queue<string> queue = new Queue<string>();
        HashSet<string> visited = new HashSet<string>();
        queue.Enqueue("0000");
        visited.Add("0000");

        int moves = 0;

        // Perform BFS
        while (queue.Count > 0) {
            int size = queue.Count;

            for (int i = 0; i < size; i++) {
                string current = queue.Dequeue();

                if (current.Equals(target)) {
                    return moves; // Target reached
                }

                // Generate all possible next combinations
                for (int j = 0; j < 4; j++) {
                    char c = current[j];

                    // Turn the wheel one step forward
                    string forward = current.Substring(0, j) + (c == '9' ? '0' : (char)(c + 1)) + current.Substring(j + 1);
                    if (!deadSet.Contains(forward) && !visited.Contains(forward)) {
                        queue.Enqueue(forward);
                        visited.Add(forward);
                    }

                    // Turn the wheel one step backward
                    string backward = current.Substring(0, j) + (c == '0' ? '9' : (char)(c - 1)) + current.Substring(j + 1);
                    if (!deadSet.Contains(backward) && !visited.Contains(backward)) {
                        queue.Enqueue(backward);
                        visited.Add(backward);
                    }
                }
            }

            moves++;
        }

        return -1; // Target is unreachable
    }
}
```

