# 752. [Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Approach: Breadth-First Search (BFS)

### Solution
```python
# Time Complexity: O(10^4 + d), where d is the size of the deadends
# Space Complexity: O(10^4 + d), for the visited set and the queue
from collections import deque

class Solution:
    def openLock(self, deadends, target):
        dead_set = set(deadends)

        if "0000" in dead_set:
            return -1  # Cannot start from the initial position

        queue = deque(["0000"])
        visited = set(["0000"])

        moves = 0

        # Perform BFS
        while queue:
            size = len(queue)

            for _ in range(size):
                current = queue.popleft()

                if current == target:
                    return moves  # Target reached

                # Generate all possible next combinations
                for j in range(4):
                    c = current[j]

                    # Turn the wheel one step forward
                    forward = current[:j] + ('0' if c == '9' else chr(ord(c) + 1)) + current[j+1:]
                    if forward not in dead_set and forward not in visited:
                        queue.append(forward)
                        visited.add(forward)

                    # Turn the wheel one step backward
                    backward = current[:j] + ('9' if c == '0' else chr(ord(c) - 1)) + current[j+1:]
                    if backward not in dead_set and backward not in visited:
                        queue.append(backward)
                        visited.add(backward)

            moves += 1

        return -1  # Target is unreachable
```

