# 834. [Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approach 1: DFS to Calculate Subtree Sizes and Initial Distance

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    tree: number[][];
    answer: number[];
    count: number[];

    constructor() {
        this.tree = [];
        this.answer = [];
        this.count = [];
    }

    sumOfDistancesInTree(N: number, edges: number[][]): number[] {
        // Initialize the tree and helper arrays
        this.tree = Array.from({ length: N }, () => []);
        this.answer = new Array(N).fill(0);
        this.count = new Array(N).fill(0);

        // Build the tree as an adjacency list
        for (let edge of edges) {
            this.tree[edge[0]].push(edge[1]);
            this.tree[edge[1]].push(edge[0]);
        }

        // Post-order DFS to calculate count and initial distances
        this.postOrder(0, -1);

        // Pre-order DFS to calculate final answer
        this.preOrder(0, -1, N);

        return this.answer;
    }

    private postOrder(node: number, parent: number) {
        // Initialize count as 1 (itself)
        this.count[node] = 1;
        for (let neighbor of this.tree[node]) {
            if (neighbor === parent) continue; // Avoid going back to parent
            this.postOrder(neighbor, node);
            // Sum up distances in subtree
            this.count[node] += this.count[neighbor];
            this.answer[node] += this.answer[neighbor] + this.count[neighbor];
        }
    }

    private preOrder(node: number, parent: number, N: number) {
        for (let neighbor of this.tree[node]) {
            if (neighbor === parent) continue; // Avoid going back to parent
            // Calculate using previously computed distances
            this.answer[neighbor] = this.answer[node] - this.count[neighbor] + (N - this.count[neighbor]);
            this.preOrder(neighbor, node, N);
        }
    }
}
```

This approach uses Depth First Search (DFS) to first calculate the sum of distances from the root to all nodes and uses it to compute the distances for the rest by adjusting them based on the tree structure.

