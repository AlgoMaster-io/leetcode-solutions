# 1514. [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approach 1: Dijkstra's Algorithm using a Priority Queue

### Solution
typescript
```typescript
// Time Complexity: O((n + m) * log n), where n is the number of nodes and m is the number of edges
// Space Complexity: O(n + m)
class Solution {
    maxProbability(n: number, edges: number[][], succProb: number[], start: number, end: number): number {
        const graph: Node[][] = Array.from({ length: n }, () => []);

        // Build adjacency list for the graph
        for (let i = 0; i < edges.length; i++) {
            const [u, v] = edges[i];
            const prob = succProb[i];
            graph[u].push(new Node(v, prob));
            graph[v].push(new Node(u, prob)); // Since the graph is undirected
        }

        const probabilities: number[] = new Array(n).fill(0); // Store max probability to each node
        probabilities[start] = 1.0; // Max probability to start node is 1

        const pq: PriorityQueue<Node> = new PriorityQueue((a, b) => b.probability - a.probability);
        pq.offer(new Node(start, 1.0));

        // Process nodes with Dijkstra's algorithm
        while (!pq.isEmpty()) {
            const current: Node = pq.poll() as Node;
            const currNode = current.node;
            const currProb = current.probability;

            // If we reach the end node, return probability
            if (currNode === end) {
                return currProb;
            }

            if (currProb < probabilities[currNode]) {
                continue;
            }

            graph[currNode].forEach((neighbor) => {
                const nextNode = neighbor.node;
                const edgeProb = neighbor.probability;

                // Calculate new probability
                const newProb = currProb * edgeProb;
                if (newProb > probabilities[nextNode]) {
                    probabilities[nextNode] = newProb;
                    pq.offer(new Node(nextNode, newProb));
                }
            });
        }

        return probabilities[end]; // Return max probability to reach end node
    }
}

class Node {
    node: number;
    probability: number;

    constructor(node: number, probability: number) {
        this.node = node;
        this.probability = probability;
    }
}

class PriorityQueue<T> {
    private data: T[];
    private comparator: (a: T, b: T) => number;

    constructor(comparator: (a: T, b: T) => number) {
        this.data = [];
        this.comparator = comparator;
    }

    offer(value: T): void {
        this.data.push(value);
        this.bubbleUp();
    }

    poll(): T | undefined {
        const max = this.data[0];
        const end = this.data.pop();
        if (this.data.length > 0 && end !== undefined) {
            this.data[0] = end;
            this.trickleDown();
        }
        return max;
    }

    isEmpty(): boolean {
        return this.data.length === 0;
    }

    private bubbleUp(): void {
        let index = this.data.length - 1;
        const element = this.data[index];

        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            const parent = this.data[parentIndex];

            if (this.comparator(element, parent) <= 0) break;
            this.data[index] = parent;
            index = parentIndex;
        }

        this.data[index] = element;
    }

    private trickleDown(): void {
        let index = 0;
        const length = this.data.length;
        const element = this.data[0];

        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let swapIndex = null;

            if (leftChildIndex < length) {
                const leftChild = this.data[leftChildIndex];
                if (this.comparator(leftChild, element) > 0) {
                    swapIndex = leftChildIndex;
                }
            }

            if (rightChildIndex < length) {
                const rightChild = this.data[rightChildIndex];
                if (
                    (swapIndex === null && this.comparator(rightChild, element) > 0) ||
                    (swapIndex !== null && this.comparator(rightChild, this.data[swapIndex]) > 0)
                ) {
                    swapIndex = rightChildIndex;
                }
            }

            if (swapIndex === null) break;
            this.data[index] = this.data[swapIndex];
            index = swapIndex;
        }

        this.data[index] = element;
    }
}
```

This problem is effectively a modification of the shortest path problem or Dijkstra's algorithm by changing distance sums to probabilities multiplied.

