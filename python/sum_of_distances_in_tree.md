# 834. [Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approach 1: DFS to Calculate Subtree Sizes and Initial Distance

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import defaultdict

class Solution:
    def sumOfDistancesInTree(self, N, edges):
        # Initialize the tree and helper arrays
        tree = defaultdict(list)
        answer = [0] * N
        count = [0] * N

        # Build the tree as an adjacency list
        for u, v in edges:
            tree[u].append(v)
            tree[v].append(u)

        # Post-order DFS to calculate count and initial distances
        def postOrder(node, parent):
            # Initialize count as 1 (itself)
            count[node] = 1
            for neighbor in tree[node]:
                if neighbor == parent:
                    continue  # Avoid going back to parent
                postOrder(neighbor, node)
                # Sum up distances in subtree
                count[node] += count[neighbor]
                answer[node] += answer[neighbor] + count[neighbor]

        # Pre-order DFS to calculate final answer
        def preOrder(node, parent, N):
            for neighbor in tree[node]:
                if neighbor == parent:
                    continue  # Avoid going back to parent
                # Calculate using previously computed distances
                answer[neighbor] = answer[node] - count[neighbor] + (N - count[neighbor])
                preOrder(neighbor, node, N)
        
        postOrder(0, -1)
        preOrder(0, -1, N)

        return answer
```

This approach uses Depth First Search (DFS) to first calculate the sum of distances from the root to all nodes and uses it to compute the distances for the rest by adjusting them based on the tree structure.

