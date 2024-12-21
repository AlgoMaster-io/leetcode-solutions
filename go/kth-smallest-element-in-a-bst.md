# [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

## Approaches
1. [Inorder Traversal with Array](#inorder-traversal-with-array)
2. [Inorder Traversal with Count](#inorder-traversal-with-count)

---

### Inorder Traversal with Array

**Intuition:**

The Binary Search Tree (BST) property mandates that an inorder traversal yields nodes in non-decreasing order. If we perform an inorder traversal, we can collect the nodes in a list and simply return the k-1 indexed element of this list as the k-th smallest element.

**Steps:**
- Conduct an inorder traversal on the BST.
- Store the visited nodes in an array.
- Return the k-1 element from the array (since array indices are zero-based).

**Time Complexity:** O(N), where N is the number of nodes in the tree, due to the necessity of traversing all nodes in the worst case.

**Space Complexity:** O(N), required to store the nodes in the list.

```go
// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func kthSmallest(root *TreeNode, k int) int {
    var inorder func(node *TreeNode)
    nums := []int{}
    
    inorder = func(node *TreeNode) {
        if node == nil {
            return
        }
        inorder(node.Left)  // Visit the left subtree
        nums = append(nums, node.Val)  // Visit the current node
        inorder(node.Right)  // Visit the right subtree
    }
    
    inorder(root)
    
    return nums[k-1]  // Return the k-th smallest element
}
```

---

### Inorder Traversal with Count

**Intuition:**

Instead of storing all the elements from the inorder traversal, we can keep a count of the nodes visited. This optimizes space as we do not need to keep all elements in memory.

**Steps:**
- Conduct an inorder traversal of the tree.
- Keep a running count of how many nodes we have visited.
- Once we've visited k nodes, we have reached our k-th smallest element.

**Time Complexity:** O(N) in the worst case for unbalanced trees where the traversal has to go through every element.

**Space Complexity:** O(H), where H is the height of the tree, corresponding to the recursion stack used during the traversal. For balanced trees this can be O(log N).

```go
// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func kthSmallest(root *TreeNode, k int) int {
    count := 0
    result := 0
    var inorder func(node *TreeNode)
    
    inorder = func(node *TreeNode) {
        if node == nil || count >= k {
            return
        }
        inorder(node.Left)  // Visit the left subtree
        count++  // Increment count after visiting each node
        if count == k {
            result = node.Val  // The k-th smallest element is found
            return
        }
        inorder(node.Right)  // Visit the right subtree
    }
    
    inorder(root)
    
    return result  // Return the k-th smallest element
}
```

This concludes the solutions. The first approach is straightforward using an additional list to store values, and the second approach optimizes this by reducing space usage, leveraging the orderly properties of the traversal to track the element count.

