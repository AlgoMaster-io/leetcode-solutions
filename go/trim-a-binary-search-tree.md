## [Leetcode 669: Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

### Approaches
- [Approach 1: Recursive Depth First Search (DFS)](#approach-1-recursive-dfs)
- [Approach 2: Iterative Solution Using a Stack](#approach-2-iterative-solution-using-a-stack)

---

### Approach 1: Recursive DFS 

The problem essentially asks us to trim parts of the binary search tree (BST) such that every node's value falls within the range [low, high]. A binary search tree (BST) inherently organizes its nodes such that for any given node, values in the left subtree are lesser and values in the right subtree are greater than the node's value. This property immensely helps in trimming nodes outside the specified range:

**Intuition:**
1. For each node, we compare its value with the given range [low, high].
2. If the node's value is less than `low`, we only need to consider nodes in the right subtree since all nodes in the left subtree will also be out of range.
3. If the node's value is greater than `high`, we only need to look at the left subtree.
4. If the node's value falls within the range, we recursively trim both left and right children.

**Recursive Function Explanation:**
- Base Case: If the current node is `nil`, return `nil`.
- If the current node's value is less than `low`, recursively trim the right subtree.
- If the current node's value is greater than `high`, recursively trim the left subtree.
- Otherwise, recursively trim both subtrees and return the current node.

```go
// TreeNode definition.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func trimBST(root *TreeNode, low int, high int) *TreeNode {
    // Recursive function to trim the BST.
    if root == nil {
        return nil
    }

    // If root's value is less than low, discard the left subtree.
    if root.Val < low {
        return trimBST(root.Right, low, high)
    }

    // If root's value is more than high, discard the right subtree.
    if root.Val > high {
        return trimBST(root.Left, low, high)
    }

    // Root is within range: Recursively trim both sides.
    root.Left = trimBST(root.Left, low, high)
    root.Right = trimBST(root.Right, low, high)

    return root
}
```

- **Time Complexity:** O(N) as we might visit every node in the tree.
- **Space Complexity:** O(N) due to the recursion stack in the worst case (unbalanced tree).

---

### Approach 2: Iterative Solution Using a Stack

**Intuition:**
Instead of using recursive calls, we can leverage an iterative approach using a stack. This method reduces recursion overhead and is useful for deep trees where recursion might hit stack limits.

**Iterative Explanation:**
1. Initialize a stack and push the root node onto it.
2. Process nodes iteratively, where for each node:
   - If it’s lesser than `low`, consider its right child.
   - If it’s greater than `high`, consider its left child.
   - Otherwise, adjust left and right children by adding them to the stack for further processing.
3. Skip nodes that fall outside the range, adjusting pointers of their parents directly.

```go
func trimBST(root *TreeNode, low int, high int) *TreeNode {
    // Dummy node to handle edge cases conveniently.
    dummy := &TreeNode{Right: root}
    stack := []*TreeNode{dummy}

    // Iteratively process the tree.
    for len(stack) > 0 {
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]

        // Process the relevant child depending on node's position relative to range.
        if node.Right != nil {
            if node.Right.Val < low {
                // Replace this subtree with its 'right' child.
                node.Right = node.Right.Right
                stack = append(stack, node)  // Reprocess this node.
            } else if node.Right.Val > high {
                // Replace this subtree with its 'left' child.
                node.Right = node.Right.Left
                stack = append(stack, node)  // Reprocess this node.
            } else {
                // Node within bounds, trim its subtrees.
                stack = append(stack, node.Right)
            }
        }

        if node.Left != nil {
            if node.Left.Val < low {
                // Replace this subtree with its 'right' child.
                node.Left = node.Left.Right
                stack = append(stack, node)  // Reprocess this node.
            } else if node.Left.Val > high {
                // Replace this subtree with its 'left' child.
                node.Left = node.Left.Left
                stack = append(stack, node)  // Reprocess this node.
            } else {
                // Node within bounds, trim its subtrees.
                stack = append(stack, node.Left)
            }
        }
    }
    return dummy.Right
}
```

- **Time Complexity:** O(N) similar to the recursive version since each node could be visited.
- **Space Complexity:** O(N) due to the stack for iterative processing.

This iterative approach directly mirrors the recursive logic but avoids call stack issues in exceptionally deep trees. It's helpful when recursion depth becomes a limitation.

