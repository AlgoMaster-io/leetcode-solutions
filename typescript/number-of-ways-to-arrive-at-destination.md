# [LeetCode Problem 1976: Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Approaches
- [Approach 1: Dijkstra's Algorithm with BFS](#approach-1-dijkstras-algorithm-with-bfs)

---

## Approach 1: Dijkstra's Algorithm with BFS

**Intuition:**

The problem of finding the number of ways to arrive at the destination can be tackled through a combination of Dijkstra's algorithm and Breadth-First Search (BFS). The goal is to find the shortest paths from the start node to all other nodes using Dijkstra's algorithm. Simultaneously, we need to count the number of ways to reach each node with the shortest path distance. 

1. **Graph Representation:** The problem is a typical shortest path problem that can be represented as a graph where intersections are nodes and roads with time as weights are edges.

2. **Priority Queue for Shortest Path:** We use a priority queue to always expand the node with the currently known shortest path, ensuring that once we settle a node with the shortest path, this path is optimal.

3. **Counting Paths:** As we perform the relaxation of edges (neighbor nodes), if we find a new shortest path, we update the shortest path length and reset the count of ways to reach the node. If we find another path with the same length, we simply add the number of ways to reach the current node to the neighbor's ways.

Here is the TypeScript code implementing this approach using Dijkstra's algorithm combined with path counting:

```typescript
function countPaths(n: number, roads: number[][]): number {
    const MOD = 1e9 + 7;
    const adjList: Array<Array<[number, number]>> = Array.from({ length: n }, () => []);
    
    // Build adjacency list representation of the graph
    roads.forEach(([src, dest, time]) => {
        adjList[src].push([dest, time]);
        adjList[dest].push([src, time]);
    });
    
    // Min-heap: stores [currentTravelTime, node]
    const minHeap: Array<[number, number]> = [[0, 0]]; // Start with node 0
    const shortestPath = new Array(n).fill(Infinity);
    shortestPath[0] = 0; // Distance to the starting node is 0
    const ways = new Array(n).fill(0);
    ways[0] = 1; // Start point has 1 way to reach itself
    
    while (minHeap.length > 0) {
        const [time, node] = minHeap.shift()!;
        
        if (time > shortestPath[node]) {
            continue;
        }
        
        // Explore neighbors
        for (const [neighbor, travelTime] of adjList[node]) {
            const totalTime = time + travelTime;
            
            // Found a shorter path to the neighbor
            if (totalTime < shortestPath[neighbor]) {
                shortestPath[neighbor] = totalTime;
                ways[neighbor] = ways[node]; // All ways to 'node' contribute to 'neighbor'
                minHeap.push([totalTime, neighbor]);
                minHeap.sort(([aTime], [bTime]) => aTime - bTime); // Keep heap properties
            }
            // Found another way to achieve the shortest path to the neighbor
            else if (totalTime === shortestPath[neighbor]) {
                ways[neighbor] = (ways[neighbor] + ways[node]) % MOD;
            }
        }
    }
    
    return ways[n - 1]; // Number of ways to reach the destination node
}
```

**Time Complexity:**  
- The time complexity is \(O((N + E) \log N)\), where \(N\) is the number of nodes and \(E\) is the number of edges.
- This is because, in Dijkstra's algorithm with a priority queue, each node and each edge is processed, and heap operations take \(O(\log N)\) time.

**Space Complexity:**  
- The space complexity is \(O(N + E)\) for storing the graph representation and the additional storage for shortest paths and ways.

