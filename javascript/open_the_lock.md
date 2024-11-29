# 752. [Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Approach: Breadth-First Search (BFS)

### Solution
```javascript
// Time Complexity: O(10^4 + d), where d is the size of the deadends
// Space Complexity: O(10^4 + d), for the visited set and the queue

function openLock(deadends, target) {
    const deadSet = new Set(deadends);

    if (deadSet.has("0000")) {
        return -1; // Cannot start from the initial position
    }

    const queue = [];
    const visited = new Set();
    queue.push("0000");
    visited.add("0000");

    let moves = 0;

    // Perform BFS
    while (queue.length > 0) {
        let size = queue.length;

        for (let i = 0; i < size; i++) {
            const current = queue.shift();

            if (current === target) {
                return moves; // Target reached
            }

            // Generate all possible next combinations
            for (let j = 0; j < 4; j++) {
                const c = current[j];

                // Turn the wheel one step forward
                const forward = current.substring(0, j) + (c === '9' ? '0' : String.fromCharCode(c.charCodeAt(0) + 1)) + current.substring(j + 1);
                if (!deadSet.has(forward) && !visited.has(forward)) {
                    queue.push(forward);
                    visited.add(forward);
                }

                // Turn the wheel one step backward
                const backward = current.substring(0, j) + (c === '0' ? '9' : String.fromCharCode(c.charCodeAt(0) - 1)) + current.substring(j + 1);
                if (!deadSet.has(backward) && !visited.has(backward)) {
                    queue.push(backward);
                    visited.add(backward);
                }
            }
        }

        moves++;
    }

    return -1; // Target is unreachable
}
```

