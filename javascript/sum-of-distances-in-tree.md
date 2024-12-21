# [Leetcode 834: Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approaches Overview
- [Approach 1: DFS with Distance Calculation](#approach-1-dfs-with-distance-calculation)
- [Approach 2: DFS with Two-Pass Technique](#approach-2-dfs-with-two-pass-technique)

### Approach 1: DFS with Distance Calculation

#### Intuition:
The problem requires calculating the sum of distances from each node to all other nodes in the tree. A naive approach might involve calculating the distance from each node to every other node individually which would be very inefficient.

However, we can use Depth-First Search (DFS) to preprocess and compute these distances systematically. We can traverse the tree from any starting node (say node 0) and compute the sum of distances to all other nodes. Once we compute these distances using one DFS traversal, we can then compute the distances for other nodes with adjustments.

#### Approach:
1. Start with any node, commonly node 0, and perform a DFS to calculate the sum of distances for that subtree rooted at the node. Store the number of nodes in each subtree.
2. With the computed subtree information, calculate the sum of distances for each node assuming that the node up to now is the root.
3. For every other node, adjust the previous computed values to find sum of distances using previously computed subtrees in O(1) time by using derived formulas.

#### Solution:
```javascript
var sumOfDistancesInTree = function(N, edges) {
    const tree = Array.from({ length: N }, () => []);
    const count = Array(N).fill(1);  // Each node is a subtree of size 1 by itself
    const res = Array(N).fill(0);

    // Build the tree from the edges list.
    for (const [u, v] of edges) {
        tree[u].push(v);
        tree[v].push(u);
    }

    // Helper function: postorder DFS to calculate the count and initial distance sum for node 0
    function postorder(node, parent) {
        for (let neighbor of tree[node]) {
            if (neighbor !== parent) {
                postorder(neighbor, node);
                count[node] += count[neighbor];
                res[0] += res[neighbor] + count[neighbor];
            }
        }
    }

    // Helper function: preorder DFS to calculate the result for each node
    function preorder(node, parent) {
        for (let neighbor of tree[node]) {
            if (neighbor !== parent) {
                // Adjust the result for the children based on the current node's result
                res[neighbor] = res[node] - count[neighbor] + (N - count[neighbor]);
                preorder(neighbor, node);
            }
        }
    }

    postorder(0, -1);
    preorder(0, -1);
    return res;
};
```

#### Complexity Analysis:
- **Time complexity**: O(N), where N is the number of nodes. Each node and edge are visited a constant number of times.
- **Space complexity**: O(N), where N is the number of nodes to store the tree and auxiliary arrays.

### Approach 2: DFS with Two-Pass Technique

#### Intuition:
This approach involves two passes of DFS: One to compute the initial distances, and a second to accumulate and adjust these distances for every other node efficiently.

#### Approach:
1. Perform a post-order DFS traversal to calculate the number of nodes and the sum of distances for the tree.
2. Use a pre-order traversal to adjust these sums and efficiently compute the required sum distances for each node.

The main innovation here is that once you know the sum of distances from one node, the distances of its neighbors can be computed in constant time using the previous subtree calculations, leveraging the recursive nature of trees.

#### Code:
The solution code remains the same since this is the optimal approach that uses the aforementioned tricks.

```javascript
var sumOfDistancesInTree = function(N, edges) {
    const tree = Array.from({ length: N }, () => []);
    const count = Array(N).fill(1);  // Each node is a subtree of size 1 by itself
    const res = Array(N).fill(0);

    for (const [u, v] of edges) {
        tree[u].push(v);
        tree[v].push(u);
    }

    function postorder(node, parent) {
        for (let neighbor of tree[node]) {
            if (neighbor !== parent) {
                postorder(neighbor, node);
                count[node] += count[neighbor];
                res[0] += res[neighbor] + count[neighbor];
            }
        }
    }

    function preorder(node, parent) {
        for (let neighbor of tree[node]) {
            if (neighbor !== parent) {
                res[neighbor] = res[node] - count[neighbor] + (N - count[neighbor]);
                preorder(neighbor, node);
            }
        }
    }

    postorder(0, -1);
    preorder(0, -1);
    return res;
};
```

#### Complexity Analysis:
- **Time complexity**: O(N), where N is the number of nodes. We perform constant-time operations per edge and node.
- **Space complexity**: O(N), where N is the number of nodes to store the tree and auxiliary arrays.

By processing the problem using this optimal two-pass DFS, we leverage the recursive property of trees to compute the required distances in an efficient manner.

