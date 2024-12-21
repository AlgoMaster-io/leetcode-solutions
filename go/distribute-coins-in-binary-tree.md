# [979. Distribute Coins in Binary Tree](https://leetcode.com/problems/distribute-coins-in-binary-tree/)

## Approaches:
1. [Brute Force - Recursive Traversal](#approach-1-brute-force---recursive-traversal)
2. [DFS with Postorder Traversal](#approach-2-dfs-with-postorder-traversal)

---

## Approach 1: Brute Force - Recursive Traversal

### Intuition:
The brute force approach involves distributing coins from each node one by one until the tree is balanced. This approach recursively traverses the entire tree, calculating the difference required for each node to equal one coin. The key observation is that at any given node, we need to ensure that it ends up with exactly one coin, and all excess coins or deficiencies should be moved up the tree to the parent. 

### Steps:
1. Define a recursive function that will process each node and calculate the excess or deficit of coins.
2. For each node, calculate the total needed moves to make the node's coins equal to one. 
3. Recursively apply the above logic to the left and right children.
4. Accumulate the moves needed at each node.

### Code:
```go
// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func distributeCoins(root *TreeNode) int {
    var totalMoves int
    
    // Recursive helper function to process each node
    var dfs func(*TreeNode) int
    dfs = func(node *TreeNode) int {
        if node == nil {
            return 0
        }
        
        // Calculate excess or deficit from left and right children
        leftExcess := dfs(node.Left)
        rightExcess := dfs(node.Right)
        
        // Total moves include adjustments needed in this subtree
        totalMoves += abs(leftExcess) + abs(rightExcess)
        
        // Return this node's net excess or deficit to parent
        return node.Val - 1 + leftExcess + rightExcess
    }
    
    dfs(root)
    return totalMoves
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

### Complexity Analysis:
- **Time Complexity:** O(N), where N is the number of nodes in the tree. We traverse each node once.
- **Space Complexity:** O(H), where H is the height of the tree. The recursion stack may go as deep as the height of the tree.

---

## Approach 2: DFS with Postorder Traversal

### Intuition:
A more efficient solution leverages a postorder depth-first search (DFS) to compute the number of moves required at each node. This approach works by thinking of the tree as a series of interconnected subtrees. By processing children nodes first (postorder traversal), the solution becomes more apparent: the coins needed to balance a subtree are the sum of excess/deficiency at the current node plus the moves required to balance its children.

### Steps:
1. Define a postorder DFS function that:
   - Computes the balance (excess or deficit) of each subtree.
   - Returns the excess/deficit of coins to be passed to its parent.
2. At each node, update total moves required using the absolute value of the excess/deficiency from children nodes.

### Code:
```go
// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func distributeCoins(root *TreeNode) int {
    var totalMoves int
    
    // Function to process the tree in postorder
    var postorderDFS func(*TreeNode) int
    postorderDFS = func(node *TreeNode) int {
        if node == nil {
            return 0
        }
        
        // Calculate for left & right, postorder means children first
        leftBalance := postorderDFS(node.Left)
        rightBalance := postorderDFS(node.Right)
        
        // Update total moves from the current subtree calculation
        totalMoves += abs(leftBalance) + abs(rightBalance)
        
        // Balance for the current node to give to its parent
        return node.Val - 1 + leftBalance + rightBalance
    }
    
    postorderDFS(root)
    return totalMoves
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

### Complexity Analysis:
- **Time Complexity:** O(N), where N is the number of nodes in the tree.
- **Space Complexity:** O(H), where H is the height of the tree, accounting for the recursion stack.

This second approach is generally more optimal and ensures clear postorder processing of the nodes to accumulate the required moves accurately.

