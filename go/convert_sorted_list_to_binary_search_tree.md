# [109. Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approach 1: Recursion with Slow and Fast Pointers

### Solution
```go
// Time Complexity: O(n log n), where n is the length of the list
// Space Complexity: O(log n) (due to recursion stack)
package main

type ListNode struct {
    Val  int
    Next *ListNode
}

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func sortedListToBST(head *ListNode) *TreeNode {
    if head == nil {
        return nil // Base case: empty list
    }
    if head.Next == nil {
        return &TreeNode{Val: head.Val} // Single node
    }

    // Find the middle of the list using slow and fast pointers
    var prev *ListNode = nil
    slow, fast := head, head
    for fast != nil && fast.Next != nil {
        prev = slow
        slow = slow.Next
        fast = fast.Next.Next
    }

    // Disconnect the left half of the list
    if prev != nil {
        prev.Next = nil
    }

    // Create the root node
    root := &TreeNode{Val: slow.Val}

    // Recursively build the left and right subtrees
    root.Left = sortedListToBST(head)
    root.Right = sortedListToBST(slow.Next)

    return root
}
```

## Approach 2: In-Order Simulation with List Conversion

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

type ListNode struct {
    Val  int
    Next *ListNode
}

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func sortedListToBST(head *ListNode) *TreeNode {
    // Convert the linked list to an array for easier access
    values := []int{}
    for head != nil {
        values = append(values, head.Val)
        head = head.Next
    }

    // Recursively construct the BST
    return buildBST(values, 0, len(values)-1)
}

func buildBST(values []int, left, right int) *TreeNode {
    if left > right {
        return nil // Base case
    }

    mid := left + (right - left) / 2 // Find the middle element
    root := &TreeNode{Val: values[mid]} // Create the root node

    // Recursively build left and right subtrees
    root.Left = buildBST(values, left, mid-1)
    root.Right = buildBST(values, mid+1, right)

    return root
}
```

