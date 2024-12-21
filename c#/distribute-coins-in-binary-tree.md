# [979. Distribute Coins in Binary Tree](https://leetcode.com/problems/distribute-coins-in-binary-tree/)

## Approaches:
- [Approach 1: Recursive DFS](#approach-1)
- [Approach 2: Postorder DFS with Efficient Coin Balancing](#approach-2)

--- 

## Approach 1: Recursive DFS

### Intuition:
The main goal is to make every node in the binary tree have exactly one coin. If a node has more or fewer coins, this disparity must be moved to its parent. The number of moves required is critically dependent on the excess or shortage of coins at each node. We can use a depth-first search (DFS) approach to calculate excess coins and add them to the move count as we traverse through the tree.

### Detailed Steps:
1. Use recursive DFS to traverse from the leaf nodes back to the root.
2. At each node, compute the excess coins as `node.val - 1` (since each node needs exactly 1 coin).
3. Accumulate this excess at its parent node, and increase the number of moves by the absolute value of this excess.
4. Continue propagating the excess upwards until reaching the root.

### Code:
```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    private int moves = 0;
    
    public int DistributeCoins(TreeNode root) {
        DFS(root);
        return moves;
    }
    
    private int DFS(TreeNode node) {
        if (node == null) return 0;

        // Recursive call to left and right children
        int leftExcess = DFS(node.left);
        int rightExcess = DFS(node.right);

        // Calculate total excess coins at current node
        int currentNodeExcess = node.val + leftExcess + rightExcess - 1;
        
        // Increase moves by the total excess moves required for current node
        moves += Math.Abs(currentNodeExcess);
        
        // Return the current node's excess to its parent
        return currentNodeExcess;
    }
}
```

### Time and Space Complexity:
- **Time Complexity**: \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is visited once.
- **Space Complexity**: \(O(H)\), where \(H\) is the height of the tree. This space is required for the recursion stack.

---

## Approach 2: Postorder DFS with Efficient Coin Balancing

### Intuition:
This approach is essentially the same as the first one but presented with an emphasis on postorder traversal. Postorder is particularly apt for problems involving accumulation from leaves to root, as we can first determine and resolve the state of child nodes before dealing with their parent.

### Detailed Steps:
1. Traverse the tree in postorder (left -> right -> node).
2. For each node, calculate the left and right subtree excess coins.
3. The coins to be moved is the absolute sum of these excess coins.
4. Return the accumulated excess to its parent.

The setup and logic are essentially very similar to the first approach but stress the ordering in which nodes are visited.

### Code:
```csharp
public class Solution {
    private int movesCount = 0;
    
    public int DistributeCoins(TreeNode root) {
        PostOrderDFS(root);
        return movesCount;
    }
    
    private int PostOrderDFS(TreeNode node) {
        if (node == null) return 0;

        // Postorder DFS
        int left = PostOrderDFS(node.left);
        int right = PostOrderDFS(node.right);

        // Compute current node excess and increment move count
        int excess = node.val + left + right - 1;
        movesCount += Math.Abs(excess);
        
        // Return this node excess to its parent
        return excess;
    }
}
```

### Time and Space Complexity:
- **Time Complexity**: \(O(N)\). Each node is processed once.
- **Space Complexity**: \(O(H)\). The space required is for the recursion due to the call stack.

Each approach here leverages DFS to solve the problem, emphasizing that the framework of using left and right subtree processing is key to solving the "Distribute Coins in Binary Tree" problem efficiently.

