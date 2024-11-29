# 752. [Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Approach: Breadth-First Search (BFS)

### Solution
```java
// Time Complexity: O(10^4 + d), where d is the size of the deadends
// Space Complexity: O(10^4 + d), for the visited set and the queue
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Set;

public class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> deadSet = new HashSet<>();
        for (String dead : deadends) {
            deadSet.add(dead);
        }

        if (deadSet.contains("0000")) {
            return -1; // Cannot start from the initial position
        }

        Queue<String> queue = new LinkedList<>();
        Set<String> visited = new HashSet<>();
        queue.add("0000");
        visited.add("0000");

        int moves = 0;

        // Perform BFS
        while (!queue.isEmpty()) {
            int size = queue.size();

            for (int i = 0; i < size; i++) {
                String current = queue.poll();

                if (current.equals(target)) {
                    return moves; // Target reached
                }

                // Generate all possible next combinations
                for (int j = 0; j < 4; j++) {
                    char c = current.charAt(j);

                    // Turn the wheel one step forward
                    String forward = current.substring(0, j) + (c == '9' ? 0 : c - '0' + 1) + current.substring(j + 1);
                    if (!deadSet.contains(forward) && !visited.contains(forward)) {
                        queue.add(forward);
                        visited.add(forward);
                    }

                    // Turn the wheel one step backward
                    String backward = current.substring(0, j) + (c == '0' ? 9 : c - '0' - 1) + current.substring(j + 1);
                    if (!deadSet.contains(backward) && !visited.contains(backward)) {
                        queue.add(backward);
                        visited.add(backward);
                    }
                }
            }

            moves++;
        }

        return -1; // Target is unreachable
    }
}
```