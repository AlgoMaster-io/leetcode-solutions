# 834. [Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approach 1: DFS to Calculate Subtree Sizes and Initial Distance

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    List<List<Integer>> tree;
    int[] answer, count;

    public int[] sumOfDistancesInTree(int N, int[][] edges) {
        // Initialize the tree and helper arrays
        tree = new ArrayList<>(N);
        answer = new int[N];
        count = new int[N];

        for (int i = 0; i < N; i++) {
            tree.add(new ArrayList<>());
        }

        // Build the tree as an adjacency list
        for (int[] edge : edges) {
            tree.get(edge[0]).add(edge[1]);
            tree.get(edge[1]).add(edge[0]);
        }

        // Post-order DFS to calculate count and initial distances
        postOrder(0, -1);

        // Pre-order DFS to calculate final answer
        preOrder(0, -1, N);

        return answer;
    }

    private void postOrder(int node, int parent) {
        // Initialize count as 1 (itself)
        count[node] = 1;
        for (int neighbor : tree.get(node)) {
            if (neighbor == parent) continue; // Avoid going back to parent
            postOrder(neighbor, node);
            // Sum up distances in subtree
            count[node] += count[neighbor];
            answer[node] += answer[neighbor] + count[neighbor];
        }
    }

    private void preOrder(int node, int parent, int N) {
        for (int neighbor : tree.get(node)) {
            if (neighbor == parent) continue; // Avoid going back to parent
            // Calculate using previously computed distances
            answer[neighbor] = answer[node] - count[neighbor] + (N - count[neighbor]);
            preOrder(neighbor, node, N);
        }
    }
}
```

This approach uses Depth First Search (DFS) to first calculate the sum of distances from the root to all nodes and uses it to compute the distances for the rest by adjusting them based on the tree structure.


