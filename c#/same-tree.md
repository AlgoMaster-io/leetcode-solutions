# [Leetcode 100: Same Tree](https://leetcode.com/problems/same-tree/)

## Solutions
- [Approach 1: Recursive Depth-First Search (DFS)](#approach-1)
- [Approach 2: Iterative Breadth-First Search (BFS) using a Queue](#approach-2)

### Approach 1: Recursive Depth-First Search (DFS)

#### Intuition
The problem essentially requires us to traverse two binary trees simultaneously and check if they are identical at every corresponding node. The recursive DFS approach is a natural fit since it allows us to directly compare two nodes and then proceed to their left and right children.

#### Steps
1. If both nodes are `null`, they are considered the same at this particular subtree.
2. If one node is `null` and the other is not, they are not the same.
3. If both nodes are not `null`, compare their values. If values differ, they are not the same.
4. Recursively check the left subtree and the right subtree.

#### Code
```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public bool IsSameTree(TreeNode p, TreeNode q) {
        // If both nodes are null, the trees are identical up to this point
        if (p == null && q == null) return true;
        
        // If one is null and the other isn't, they aren't identical
        if (p == null || q == null) return false;
        
        // If the values of current nodes aren't the same, not identical
        if (p.val != q.val) return false;
        
        // Recursively check both left and right subtrees
        return IsSameTree(p.left, q.left) && IsSameTree(p.right, q.right);
    }
}
```

#### Time Complexity
- O(min(N, M)), where N and M are the number of nodes in the two trees.
  
#### Space Complexity
- O(min(H1, H2)), where H1 and H2 are the heights of the trees resulting from the function call stack.

### Approach 2: Iterative Breadth-First Search (BFS) using a Queue

#### Intuition
Another method to tackle this problem is by using a queue to iteratively check each corresponding node of the two trees. BFS iteratively processes nodes in layers and can effectively be used to compare nodes at the same depth simultaneously.

#### Steps
1. Initialize a queue that will store tuples of nodes (one from each tree).
2. While the queue is not empty:
   - Dequeue a pair of nodes.
   - If both are `null`, continue.
   - If one is `null` or their values differ, return `false`.
   - Enqueue the left children of the pair, then enqueue the right children.
3. If every pair of nodes passes the checks, return `true`.

#### Code
```csharp
using System.Collections.Generic;

public class Solution {
    public bool IsSameTree(TreeNode p, TreeNode q) {
        // Use a queue to help with the level order traversal
        Queue<(TreeNode, TreeNode)> queue = new Queue<(TreeNode, TreeNode)>();
        queue.Enqueue((p, q));
        
        while (queue.Count > 0) {
            // Dequeue a pair of nodes
            var (node1, node2) = queue.Dequeue();
            
            // If both nodes are null, continue to the next pair
            if (node1 == null && node2 == null) continue;
            
            // If one is null or values are not the same, they're not identical
            if (node1 == null || node2 == null || node1.val != node2.val) return false;
            
            // Enqueue their left children
            queue.Enqueue((node1.left, node2.left));
            // Enqueue their right children
            queue.Enqueue((node1.right, node2.right));
        }
        
        // Trees are identical
        return true;
    }
}
```

#### Time Complexity
- O(min(N, M)), where N and M are the number of nodes in the two trees.
  
#### Space Complexity
- O(min(N, M)) for the space needed to store the queue. This can grow up to the maximum breadth of the trees.

