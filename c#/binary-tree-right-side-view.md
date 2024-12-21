## [Leetcode 199: Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

### Approaches:
1. [Level Order Traversal with Queue](#level-order-traversal-with-queue)
2. [DFS with Level Tracking](#dfs-with-level-tracking)

### Approach 1: Level Order Traversal with Queue

**Intuition**:  
We can solve the problem using a level order traversal (BFS) approach. For each level of the tree, the rightmost element is the one visible from the right side. We keep a track of the last element in each level by using a queue and always take the last element that we encounter at each level.

**Steps**:
- Perform a level order traversal using a queue.
- For each level, track the last node value that is visited. This value will be the rightmost node for that level.
- Add this to the result list of right-side view nodes.

**Time Complexity**: O(n) - We visit each node exactly once.  
**Space Complexity**: O(n) - The space required for the queue.

```csharp
public IList<int> RightSideView(TreeNode root) {
    List<int> rightView = new List<int>();
    
    if (root == null) {
        return rightView;
    }
    
    Queue<TreeNode> queue = new Queue<TreeNode>();
    queue.Enqueue(root);
    
    while (queue.Count > 0) {
        int levelSize = queue.Count;
        int rightMostValue = 0;
        
        // Traverse the current level
        for (int i = 0; i < levelSize; i++) {
            TreeNode currentNode = queue.Dequeue();
            rightMostValue = currentNode.val;
            
            // Add left and right children to the queue
            if (currentNode.left != null) {
                queue.Enqueue(currentNode.left);
            }
            if (currentNode.right != null) {
                queue.Enqueue(currentNode.right);
            }
        }
        
        // The last value visited in this level is the rightmost one
        rightView.Add(rightMostValue);
    }
    
    return rightView;
}
```

### Approach 2: DFS with Level Tracking

**Intuition**:  
We can also solve the problem using Depth First Search (DFS) by ensuring that we visit the right side of each level first. We keep track of the maximum depth visited and add the node’s value to the result when we first reach a new deeper level.

**Steps**:
- Implement a DFS function with tracking of the current level.
- If the current level is greater than the maximum we've visited so far, add the node’s value to the result.
- Traverse the right child first so that its value is considered first if it exists on the current level.

**Time Complexity**: O(n) - We visit each node exactly once.  
**Space Complexity**: O(n) - The recursion stack in the worst case (for a skewed tree).

```csharp
public IList<int> RightSideView(TreeNode root) {
    List<int> rightView = new List<int>();
    
    void DFS(TreeNode node, int currentLevel) {
        if (node == null) {
            return;
        }
        
        // If this is the first time visiting this depth, add the node value
        if (currentLevel == rightView.Count) {
            rightView.Add(node.val);
        }
        
        // Explore the right side first
        DFS(node.right, currentLevel + 1);
        DFS(node.left, currentLevel + 1);
    }
    
    DFS(root, 0);
    return rightView;
}
```

Both of these solutions provide a clear path for seeing the right side view of a binary tree using different but effective tree traversal methods. The BFS method is intuitive and systematic for each level of the tree, while DFS offers a quick way to reach deeper levels efficiently.

