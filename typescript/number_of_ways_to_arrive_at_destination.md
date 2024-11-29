
# 1976. [Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Approach 1: Dijkstra's Algorithm with Modification

### Solution
typescript
```typescript
// Time Complexity: O(E log V) where E is the number of edges and V is the number of vertices
// Space Complexity: O(V)

function countPaths(n: number, roads: number[][]): number {
    const MOD = 1_000_000_007;
    const graph: Array<Array<[number, number]>> = Array.from({ length: n }, () => []);

    // Construct the graph
    for (const road of roads) {
        const [u, v, time] = road;
        graph[u].push([v, time]);
        graph[v].push([u, time]);
    }

    // Dijkstra's algorithm modifications
    const minDist = new Array(n).fill(Infinity);
    const ways = new Array(n).fill(0);
    ways[0] = 1;
    minDist[0] = 0;

    const pq = new MinPriorityQueue<{ priority: number, element: number }>();
    pq.enqueue(0, 0); // {distance, node}

    while (!pq.isEmpty()) {
        const current = pq.dequeue();
        const currentDist = current.priority;
        const u = current.element;

        if (currentDist > minDist[u]) continue;

        // Traverse neighbors
        for (const [v, time] of graph[u]) {
            // Relaxation
            if (minDist[u] + time < minDist[v]) {
                minDist[v] = minDist[u] + time;
                ways[v] = ways[u];
                pq.enqueue(v, minDist[v]);
            } else if (minDist[u] + time === minDist[v]) {
                ways[v] = (ways[v] + ways[u]) % MOD;
            }
        }
    }

    return ways[n - 1];
}

class MinPriorityQueue<T> {
    private heap: Array<{ element: T, priority: number }> = [];

    enqueue(element: T, priority: number): void {
        this.heap.push({ element, priority });
        this.bubbleUp();
    }

    dequeue(): { element: T, priority: number } {
        const min = this.heap[0];
        const end = this.heap.pop();
        if (this.heap.length > 0 && end) {
            this.heap[0] = end;
            this.sinkDown();
        }
        return min!;
    }

    isEmpty(): boolean {
        return this.heap.length === 0;
    }

    private bubbleUp(): void {
        let idx = this.heap.length - 1;
        const element = this.heap[idx];
        while (idx > 0) {
            const parentIdx = Math.floor((idx - 1) / 2);
            const parent = this.heap[parentIdx];
            if (element.priority >= parent.priority) break;
            this.heap[idx] = parent;
            this.heap[parentIdx] = element;
            idx = parentIdx;
        }
    }

    private sinkDown(): void {
        let idx = 0;
        const length = this.heap.length;
        const element = this.heap[0];
        while (true) {
            const leftChildIdx = 2 * idx + 1;
            const rightChildIdx = 2 * idx + 2;
            let leftChild, rightChild;
            let swap = null;

            if (leftChildIdx < length) {
                leftChild = this.heap[leftChildIdx];
                if (leftChild.priority < element.priority) {
                    swap = leftChildIdx;
                }
            }
            if (rightChildIdx < length) {
                rightChild = this.heap[rightChildIdx];
                if (
                    (swap === null && rightChild.priority < element.priority) ||
                    (swap !== null && rightChild.priority < leftChild!.priority)
                ) {
                    swap = rightChildIdx;
                }
            }
            if (swap === null) break;
            this.heap[idx] = this.heap[swap];
            this.heap[swap] = element;
            idx = swap;
        }
    }
}
```

This solution efficiently finds the number of shortest paths from node 0 to node n-1 using a modified version of Dijkstra's algorithm that keeps track of the number of ways to arrive at each node. It uses a priority queue to explore the shortest paths and maintains an array to store the number of ways to reach each node at the shortest distance.

