
# [Leetcode 752: Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Approaches

1. [Breadth-First Search (BFS)](#breadth-first-search-bfs)

## Breadth-First Search (BFS)

### Intuition
The Open the Lock problem can be visualized as a shortest path problem on an unweighted graph, where each lock combination is a node, and each move to an adjacent combination (by rotating one wheel) represents an edge. A Breadth-First Search (BFS) is well-suited for finding the shortest path in unweighted graphs, as it explores all nodes at the present "depth" before moving on to nodes at the next depth.

### Solution

In BFS, we utilize a queue to explore nodes layer by layer:
- Start with the initial node ("0000") and add it to the queue.
- For the current node, enumerate all possible ways to move to adjacent nodes by turning each wheel one position forward or backward.
- Add each valid adjacent node to the queue if it hasn't been visited and isn't a deadend.
- Keep track of visited nodes to avoid cycles and redundant work.

### Implementation

```typescript
function openLock(deadends: string[], target: string): number {
    // Special cases: if starting point or target is unreachable
    if (target === "0000") return 0;
    if (deadends.includes("0000")) return -1;
  
    // Set of deadends for quick lookup
    let deadSet = new Set(deadends);
    let seen = new Set<string>();
    let queue: [string, number][] = [["0000", 0]]; // Tuple consists of the combination string and the number of moves
  
    // Helper function to generate all possible moves from current state
    function getNeighbors(cur: string): string[] {
        let neighbors: string[] = [];
        for (let i = 0; i < 4; i++) {
            let digit = Number(cur[i]);
            // Each wheel can turn +1 or -1
            for (let move of [1, -1]) {
                let newDigit = (digit + move + 10) % 10;  // Wrap around using modulo
                // Generate new combination
                let newCombination = cur.substring(0, i) + newDigit + cur.substring(i + 1);
                neighbors.push(newCombination);
            }
        }
        return neighbors;
    }
  
    while (queue.length > 0) {
        let [current, steps] = queue.shift()!;
    
        // If we find the target, return the number of steps taken to reach it
        if (current === target) {
            return steps;
        }
      
        // Avoid processing deadends or revisiting seen combinations
        if (deadSet.has(current) || seen.has(current)) {
            continue;
        }
      
        seen.add(current);
      
        // Queue up all neighbors (next possible combinations)
        for (let neighbor of getNeighbors(current)) {
            if (!seen.has(neighbor) && !deadSet.has(neighbor)) {
                queue.push([neighbor, steps + 1]);
            }
        }
    }
    // If we exhaust the queue without finding the target, there's no solution
    return -1;
}
```

### Time Complexity
- The time complexity is O(10^4), where 10^4 represents all possible combinations (0000 to 9999). However, due to pruning with deadends and visited nodes, in practice, it will be much lower.

### Space Complexity
- The space complexity is O(10^4) due to the queue and visited set potentially holding all possible combinations in the worst-case scenario.

