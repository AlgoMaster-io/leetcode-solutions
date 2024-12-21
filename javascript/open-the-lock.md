# [Leetcode 752: Open the Lock](https://leetcode.com/problems/open-the-lock/)

### Approaches
- [Approach 1: Breadth-First Search (BFS)](#approach-1-breadth-first-search-bfs)

## Approach 1: Breadth-First Search (BFS)

### Intuition
The problem is essentially a shortest path problem on an unweighted graph. The vertices of the graph are the different lock combinations, and the edges exist between combinations that can be reached by turning a single wheel. BFS is a suitable algorithm for finding the shortest path in an unweighted graph. We will treat each lock combination as a node and represent edges as valid one-move transitions between these nodes. We'll use BFS to explore all possible paths from the initial lock configuration "0000" and return the shortest path to the target configuration by keeping track of the number of moves.

### Steps
1. Initialize a queue with the starting node "0000" and a step count of 0.
2. Use a set to keep track of visited lock combinations to avoid re-processing.
3. For each combination:
   - Generate all possible valid lock combinations that are one move away.
   - Check if any of these are the target combination.
   - Mark the new combinations as visited and add them to the queue for further exploration.
4. If the queue is exhausted without finding the target, return -1 indicating it's unreachable.

### Implementation

```javascript
function openLock(deadends, target) {
    // Convert deadends to a set for O(1) lookup.
    const deadset = new Set(deadends);
    // Track visited combinations to prevent re-visiting.
    const visited = new Set();
    // Queue for BFS, starts with the initial lock state and 0 moves.
    const queue = [['0000', 0]];

    // Start BFS.
    while (queue.length) {
        const [current, steps] = queue.shift();

        // If current combination is a deadend, continue to the next.
        if (deadset.has(current)) continue;

        // If we reach the target, return the number of steps.
        if (current === target) return steps;

        for (const neighbor of getNeighbors(current)) {
            if (!visited.has(neighbor)) {
                visited.add(neighbor);
                queue.push([neighbor, steps + 1]);
            }
        }
    }

    // If target is unreachable, return -1.
    return -1;
}

function getNeighbors(node) {
    const neighbors = [];
    for (let i = 0; i < 4; i++) {
        const digit = parseInt(node[i]);
        // For each wheel position generate the combinations by moving up or down.
        for (const diff of [-1, 1]) {
            const newDigit = (digit + diff + 10) % 10;
            const newCombination = node.slice(0, i) + newDigit + node.slice(i + 1);
            neighbors.push(newCombination);
        }
    }
    return neighbors;
}
```

### Time Complexity
The time complexity of this approach is O(b^d), where b is the branch factor (8 possible neighbors per node after avoiding deadends), and d is the number of steps (depth in our case).

### Space Complexity
The space complexity is O(b^d) due to the queue space. Likewise, the space needed to maintain the visited set has an upper bound of 10,000 if all possible combinations are explored.

By using a BFS approach, you efficiently search for the shortest path from the starting lock combination to the target without unnecessary explorations.

