# [Leetcode 103: Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approaches
- [Approach 1: Breadth-First Search (BFS) using Queues and List Reversal](#approach-1)
- [Approach 2: BFS with Deque for Zigzag Control](#approach-2)

## Approach 1: Breadth-First Search (BFS) using Queues and List Reversal

### Intuition:
The problem requires alternating the order of traversal at each depth of the binary tree (left-to-right, then right-to-left, and so on). This can be achieved by using a Queue for level order traversal, and using an iteration counter to decide when to reverse the level list.

### Steps:
1. Use a Queue to perform a level-order traversal of the tree.
2. At each level, store the node values in a list.
3. Use a flag to decide whether to reverse the current level's list, simulating the zigzag pattern.
4. Add the processed list to the final result.
5. Continue until all levels are processed.

### Implementation:
```csharp
public class Solution {
    public IList<IList<int>> ZigzagLevelOrder(TreeNode root) {
        var result = new List<IList<int>>();
        if (root == null) return result;
        
        var queue = new Queue<TreeNode>();
        queue.Enqueue(root);
        bool leftToRight = true;

        while (queue.Count > 0) {
            int size = queue.Count;
            var level = new List<int>();

            // Process all nodes at the current level
            for (int i = 0; i < size; i++) {
                var node = queue.Dequeue();
                level.Add(node.val);
                if (node.left != null) queue.Enqueue(node.left);
                if (node.right != null) queue.Enqueue(node.right);
            }

            // Reverse the level list if needed
            if (!leftToRight) level.Reverse();
            result.Add(level);
            leftToRight = !leftToRight; // Toggle the direction for the next level
        }
        
        return result;
    }
}
```

### Complexity Analysis:
- **Time Complexity:** O(N), where N is the number of nodes in the tree. Each node is processed exactly once.
- **Space Complexity:** O(N), to store the output list and the queue in the worst case.

## Approach 2: BFS with Deque for Zigzag Control

### Intuition:
Rather than reversing the list at each level, you can optimize the solution by using a Deque. You can add nodes to the list from the front or back based on the current direction.

### Steps:
1. Utilize a Deque to allow for both front and back operations.
2. Toggle between left-to-right and right-to-left order using the Deque's `AddFirst` or `AddLast` methods.
3. Process each level, and build the resultant list directly without reversing.

### Implementation:
```csharp
public class Solution {
    public IList<IList<int>> ZigzagLevelOrder(TreeNode root) {
        var result = new List<IList<int>>();
        if (root == null) return result;

        var queue = new Queue<TreeNode>();
        queue.Enqueue(root);
        bool leftToRight = true;

        while (queue.Count > 0) {
            int size = queue.Count;
            var level = new LinkedList<int>();

            for (int i = 0; i < size; i++) {
                var node = queue.Dequeue();

                // Depending on the direction, add the node value either at the start or the end
                if (leftToRight) {
                    level.AddLast(node.val);
                } else {
                    level.AddFirst(node.val);
                }

                // Enqueue child nodes for the next level
                if (node.left != null) queue.Enqueue(node.left);
                if (node.right != null) queue.Enqueue(node.right);
            }

            result.Add(new List<int>(level));
            leftToRight = !leftToRight;
        }
        
        return result;
    }
}
```

### Complexity Analysis:
- **Time Complexity:** O(N), as each node is visited once.
- **Space Complexity:** O(N), for storing the result and using additional Deque for the current level elements.

These approaches efficiently handle zigzag level order traversal by exploiting properties of data structures like Queues and Deques. The use of a Deque specifically optimizes operations to avoid reversing lists, resulting in cleaner and potentially faster execution, especially for larger data sets.

