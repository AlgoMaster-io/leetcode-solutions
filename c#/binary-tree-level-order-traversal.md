## [Leetcode 102: Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

### Table of Contents
- [Approach 1: BFS using a Queue](#approach-1-bfs-using-a-queue)
- [Approach 2: DFS using Recursion](#approach-2-dfs-using-recursion)

---

### Approach 1: BFS using a Queue

#### Intuition
To perform a level order traversal, we can use a Breadth-First Search (BFS). BFS is ideal for level-wise traversal as it explores nodes level by level. By using a queue data structure, we can traverse the tree level by level and collect all nodes at the same level before proceeding to the next.

#### Steps:
1. Initialize an empty list `result` to store the level order traversal.
2. Use a queue initialized with the root node.
3. While the queue is not empty:
   - Determine the number of nodes at the current level.
   - Extract nodes at the current level from the queue, recording their values in a list `currentLevel`.
   - Add the children of extracted nodes to the queue for the next level.
4. Append the `currentLevel` list to the `result`.

#### Code
```csharp
public class Solution {
    public IList<IList<int>> LevelOrder(TreeNode root) {
        IList<IList<int>> result = new List<IList<int>>();
        if (root == null) return result; // Return immediately if tree is empty
        
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root); // Start with the root
        
        while (queue.Count > 0) {
            int levelSize = queue.Count; // Number of nodes at current level
            List<int> currentLevel = new List<int>();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.Dequeue(); // Get a node from the current level
                currentLevel.Add(currentNode.val);
                
                // Add child nodes of the current node for the next level
                if (currentNode.left != null) queue.Enqueue(currentNode.left);
                if (currentNode.right != null) queue.Enqueue(currentNode.right);
            }
            
            result.Add(currentLevel); // Store the current level in the result
        }
        
        return result;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is processed exactly once.
- **Space Complexity**: O(n) in the worst case where the last level contains n/2 nodes.

---

### Approach 2: DFS using Recursion

#### Intuition
Instead of using BFS, DFS can also be used to perform a level order traversal by tracking depths during the recursive traversal. Each recursive call proceeds down one branch before moving to another, but by augmenting it to add nodes to a list corresponding to their level, we can simulate level order functionality.

#### Steps:
1. Utilize a helper function `dfs` that:
   - Adds the node's value to a `result` list based on the current depth.
   - Recursively calls itself for left and right children with increased depth.
2. Start the recursion with the root and an initial depth of 0.

#### Code
```csharp
public class Solution {
    public IList<IList<int>> LevelOrder(TreeNode root) {
        IList<IList<int>> result = new List<IList<int>>();
        dfs(root, 0, result);
        return result;
    }
    
    private void dfs(TreeNode node, int depth, IList<IList<int>> result) {
        if (node == null) return; // Base case: if node is null
        
        // If this is the first time we reach this depth, create a new list
        if (depth == result.Count) {
            result.Add(new List<int>());
        }
        
        // Add the current node's value at the current depth
        result[depth].Add(node.val);
        
        // Recurse on left and right children
        dfs(node.left, depth + 1, result);
        dfs(node.right, depth + 1, result);
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is visited exactly once.
- **Space Complexity**: O(n) for the call stack, potentially O(n) for the result list.

