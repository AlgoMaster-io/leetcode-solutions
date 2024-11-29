# 1514. [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approach 1: Dijkstra's Algorithm using a Priority Queue

### Solution

```javascript
// Time Complexity: O((n + m) * log n), where n is the number of nodes and m is the number of edges
// Space Complexity: O(n + m)
class Solution {
    maxProbability(n, edges, succProb, start, end) {
        // Initialize graph
        const graph = Array.from({ length: n }, () => []);

        // Build adjacency list for the graph
        for (let i = 0; i < edges.length; i++) {
            const [u, v] = edges[i];
            const prob = succProb[i];
            graph[u].push({ node: v, probability: prob });
            graph[v].push({ node: u, probability: prob }); // Since the graph is undirected
        }

        const probabilities = Array(n).fill(0); // Store max probability to each node
        probabilities[start] = 1.0; // Max probability to start node is 1

        const pq = new PriorityQueue((a, b) => b.probability - a.probability);
        pq.offer({ node: start, probability: 1.0 });

        // Process nodes with Dijkstra's algorithm
        while (!pq.isEmpty()) {
            const current = pq.poll();
            const { node: currNode, probability: currProb } = current;

            // If we reach the end node, return probability
            if (currNode === end) {
                return currProb;
            }

            if (currProb < probabilities[currNode]) {
                continue;
            }

            for (const neighbor of graph[currNode]) {
                const { node: nextNode, probability: edgeProb } = neighbor;

                // Calculate new probability
                const newProb = currProb * edgeProb;
                if (newProb > probabilities[nextNode]) {
                    probabilities[nextNode] = newProb;
                    pq.offer({ node: nextNode, probability: newProb });
                }
            }
        }

        return probabilities[end]; // Return max probability to reach end node
    }
}

class PriorityQueue {
    constructor(comparator) {
        this.comparator = comparator;
        this.elements = [];
    }

    offer(element) {
        this.elements.push(element);
        this.heapifyUp(this.elements.length - 1);
    }

    poll() {
        const maxElement = this.elements[0];
        const endElement = this.elements.pop();
        if (this.elements.length > 0) {
            this.elements[0] = endElement;
            this.heapifyDown(0);
        }
        return maxElement;
    }

    isEmpty() {
        return this.elements.length === 0;
    }

    heapifyUp(index) {
        const { elements, comparator } = this;
        let currentIndex = index;
        while (currentIndex > 0) {
            const parentIndex = Math.floor((currentIndex - 1) / 2);
            if (comparator(elements[currentIndex], elements[parentIndex]) > 0) {
                [elements[currentIndex], elements[parentIndex]] = [elements[parentIndex], elements[currentIndex]];
                currentIndex = parentIndex;
            } else {
                break;
            }
        }
    }

    heapifyDown(index) {
        const { elements, comparator } = this;
        const length = elements.length;
        let currentIndex = index;
        while (true) {
            const leftIndex = 2 * currentIndex + 1;
            const rightIndex = 2 * currentIndex + 2;
            let largestIndex = currentIndex;

            if (leftIndex < length && comparator(elements[leftIndex], elements[largestIndex]) > 0) {
                largestIndex = leftIndex;
            }

            if (rightIndex < length && comparator(elements[rightIndex], elements[largestIndex]) > 0) {
                largestIndex = rightIndex;
            }

            if (largestIndex === currentIndex) {
                break;
            }

            [elements[currentIndex], elements[largestIndex]] = [elements[largestIndex], elements[currentIndex]];
            currentIndex = largestIndex;
        }
    }
}
```

This problem is effectively a modification of the shortest path problem or Dijkstra's algorithm by changing distance sums to probabilities multiplied.

