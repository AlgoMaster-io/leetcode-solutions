# [Sum of Distances in Tree - Leetcode 834](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Post-Order and Pre-Order DFS](#approach-2-post-order-and-pre-order-dfs)

---

## Approach 1: Brute Force

#### Intuition:
The most straightforward approach is to calculate the distance from each node to every other node by performing a depth-first search (DFS) from each node. This method is intuitive but inefficient for large trees.

#### Steps:
1. For each node in the tree, perform a DFS to calculate the sum of distances to all other nodes.
2. Store the results and continue to the next node.
3. Return the list of calculated distances.

#### Code:

```typescript
function sumOfDistancesInTreeBruteForce(n: number, edges: number[][]): number[] {
    const graph = Array.from({length: n}, () => []);
    for (const [u, v] of edges) {
        graph[u].push(v);
        graph[v].push(u);
    }

    const result = Array(n).fill(0);

    function dfs(node: number, parent: number, distance: number): number {
        let totalDistance = distance;
        for (const neighbor of graph[node]) {
            if (neighbor !== parent) {
                totalDistance += dfs(neighbor, node, distance + 1);
            }
        }
        return totalDistance;
    }

    for (let i = 0; i < n; i++) {
        result[i] = dfs(i, -1, 0);
    }

    return result;
}
```

#### Time and Space Complexity:
- **Time Complexity**: \(O(n^2)\) - DFS from each node results in quadratic time complexity.
- **Space Complexity**: \(O(n)\) - Storage for graph and result array.

---

## Approach 2: Post-Order and Pre-Order DFS

#### Intuition:
We can optimize the solution by using dynamic programming combined with two depth-first searches (DFS). The first DFS is a post-order traversal to calculate the subtree sizes and initial results of distances. The second DFS is a pre-order traversal to compute the result for each node using previously calculated values.

#### Steps:
1. **Post-order DFS**: Starting from an arbitrary root, calculate the initial distance sum for the root node and number of nodes in each subtree.
2. **Pre-order DFS**: Use results from the root computation to calculate results for other nodes efficiently.

#### Code:

```typescript
function sumOfDistancesInTree(n: number, edges: number[][]): number[] {
    const graph = Array.from({length: n}, () => []);
    for (const [u, v] of edges) {
        graph[u].push(v);
        graph[v].push(u);
    }

    const count = Array(n).fill(1);
    const result = Array(n).fill(0);

    function postOrder(node: number, parent: number): void {
        for (const neighbor of graph[node]) {
            if (neighbor !== parent) {
                postOrder(neighbor, node);
                count[node] += count[neighbor];
                result[node] += result[neighbor] + count[neighbor];
            }
        }
    }

    function preOrder(node: number, parent: number): void {
        for (const neighbor of graph[node]) {
            if (neighbor !== parent) {
                result[neighbor] = result[node] - count[neighbor] + (n - count[neighbor]);
                preOrder(neighbor, node);
            }
        }
    }

    postOrder(0, -1);
    preOrder(0, -1);

    return result;
}
```

#### Time and Space Complexity:
- **Time Complexity**: \(O(n)\) - Each node and edge is visited a constant number of times.
- **Space Complexity**: \(O(n)\) - Storage for graph, count array, and result array.

This approach efficiently computes the sum of distances using dynamic programming by leveraging information from parent nodes to child nodes, reducing redundant calculations.

