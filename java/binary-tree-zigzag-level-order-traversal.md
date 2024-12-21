# [Binary Tree Zigzag Level Order Traversal - Leetcode 103](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approaches
- [Breadth-First Search (BFS) using Deque](#bfs-using-deque)
- [Breadth-First Search (BFS) with Two Stacks](#bfs-with-two-stacks)

---

### BFS using Deque

**Intuition:**
The zigzag level order traversal requires alternating the direction of each level while we traverse the binary tree. By utilizing a `Deque`, we can alternate between adding levels to the front or the back, effectively reversing the order of nodes as needed.

**Algorithm:**
1. Initialize a `Deque` to serve as a queue for BFS.
2. Use a boolean `leftToRight` to keep track of the direction of the traversal.
3. For each level, determine its size, and traverse the nodes. Depending on `leftToRight`, we either add nodes to a list normally or reverse them.
4. Switch the direction for the next level.
5. Add the completed levels to the results and return once all levels are processed.

**Java Code:**
```java
import java.util.*;

class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        // Initialize a deque for BFS traversal
        Deque<TreeNode> queue = new ArrayDeque<>();
        // Start with the root node
        queue.offer(root);
        // This boolean toggles traversal direction
        boolean leftToRight = true;
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> currentLevel = new ArrayList<>(size);
            // Process all nodes at the current level
            for (int i = 0; i < size; i++) {
                TreeNode currentNode = queue.poll();
                // Add the node's value in the direction specified
                if (leftToRight) {
                    currentLevel.add(currentNode.val);
                } else {
                    currentLevel.add(0, currentNode.val);
                }
                // Add left and right children to the deque
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
            // Add the level to the result list
            result.add(currentLevel);
            // Reverse the direction
            leftToRight = !leftToRight;
        }
        
        return result;
    }
}
```

**Time Complexity:** O(N), where N is the number of nodes in the tree, since we traverse each node once.

**Space Complexity:** O(N), needed for the `queue` which stores nodes at the current level. The largest number of nodes at any level is O(N/2) in a balanced tree.

---

### BFS with Two Stacks

**Intuition:**
Using two stacks allows us to manage two levels of processing: one for the current level and another for the next, enabling us to control the processing order explicitly.

**Algorithm:**
1. Use two stacks: one for current processing (`currentLevel`) and another for the next level (`nextLevel`).
2. Toggle the direction of node addition to the `nextLevel` stack with a boolean `leftToRight`.
3. Pop nodes from `currentLevel`, and based on the direction, push their children onto `nextLevel` in appropriate order.
4. When `currentLevel` is empty, swap stacks and repeat until all nodes are processed.

**Java Code:**
```java
import java.util.*;

class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        // Two stacks for alternating levels
        Stack<TreeNode> currentLevel = new Stack<>();
        Stack<TreeNode> nextLevel = new Stack<>();
        currentLevel.push(root);
        boolean leftToRight = true;
        
        List<Integer> currentList = new ArrayList<>();
        
        while (!currentLevel.isEmpty()) {
            TreeNode currentNode = currentLevel.pop();
            currentList.add(currentNode.val);
            
            // Add children to nextLevel stack in the correct order
            if (leftToRight) {
                if (currentNode.left != null) {
                    nextLevel.push(currentNode.left);
                }
                if (currentNode.right != null) {
                    nextLevel.push(currentNode.right);
                }
            } else {
                if (currentNode.right != null) {
                    nextLevel.push(currentNode.right);
                }
                if (currentNode.left != null) {
                    nextLevel.push(currentNode.left);
                }
            }
            
            if (currentLevel.isEmpty()) {
                // End of current level
                result.add(new ArrayList<>(currentList));
                currentList.clear();
                // Swap stacks
                leftToRight = !leftToRight;
                Stack<TreeNode> temp = currentLevel;
                currentLevel = nextLevel;
                nextLevel = temp;
            }
        }
        
        return result;
    }
}
```

**Time Complexity:** O(N), as every node is processed once from the stacks.

**Space Complexity:** O(N), for the space used by the stacks, which at most holds the nodes at one level.

