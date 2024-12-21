# [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approaches
1. [Priority Queue (Dijkstra's Algorithm)](#1-priority-queue-dijkstras-algorithm)

---

## 1. Priority Queue (Dijkstra's Algorithm)

### Intuition
The problem of finding a path with the maximum probability can be approached similarly to finding the shortest path problem in graphs, where instead of minimizing path length, we maximize the probability. Dijkstra's algorithm is a perfect fit here. We'll use a max heap (priority queue) to explore the most probable paths first.

### Steps
1. Initialize a priority queue with the starting node and a probability of 1.
2. Maintain an array to keep track of the maximum probability to reach each node starting from the source.
3. Explore the graph using the priority queue, at each step considering paths with the highest probability.
4. For each node explored, update the probabilities of neighboring nodes if a higher probability path is found.
5. Continue until the target node is reached or all possible paths are explored.

### Detailed Code

```typescript
function maxProbability(n: number, edges: number[][], succProb: number[], start: number, end: number): number {
    // Create an adjacency list for the graph where each edge has a probability
    const graph: Map<number, number[]>[] = new Array(n).fill(null).map(() => new Map());
    for (let i = 0; i < edges.length; i++) {
        const [a, b] = edges[i];
        const prob = succProb[i];
        graph[a].set(b, prob);
        graph[b].set(a, prob);
    }

    // Priority queue to store (probability, node) pairs
    const queue: [number, number][] = [[1, start]];
    
    // Array to store the maximum probability to reach each node
    const probabilities: number[] = new Array(n).fill(0);
    probabilities[start] = 1;

    while (queue.length > 0) {
        // Extract the node with the highest probability
        const [currentProb, current] = queue.pop()!;
        
        if (current === end) return currentProb; // If we reached the target node, return its probability

        // Explore neighbors
        for (const [next, prob] of graph[current]) {
            // Calculate new probability to reach the neighbor
            const newProb = currentProb * prob;

            // If this path gives a higher probability, we update and push onto the queue
            if (newProb > probabilities[next]) {
                probabilities[next] = newProb;
                queue.push([newProb, next]);
            }
        }

        // Sort the queue to work as a priority queue (max heap)
        queue.sort((a, b) => b[0] - a[0]);
    }

    return 0; // Return 0 if there's no path from start to end
}
```

### Complexity Analysis
- **Time Complexity**: \(O(E \log V)\)
  - Where \(E\) is the number of edges. The priority queue operation takes \(\log V\) and each edge is pushed once.
- **Space Complexity**: \(O(V + E)\)
  - To store the graph and priority queue, where \(V\) is the number of vertices and \(E\) is the number of edges in the graph.

This approach ensures that we efficiently explore paths with the highest probability first, similar to Dijkstra’s algorithm but tailored to maximize multiplicative probabilities instead of additive costs.

