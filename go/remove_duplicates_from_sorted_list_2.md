# [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## Approach: Dummy Node and Two Pointers

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)

type ListNode struct {
    Val  int
    Next *ListNode
}

func deleteDuplicates(head *ListNode) *ListNode {
    dummy := &ListNode{0, head} // Dummy node to handle edge cases
    prev := dummy // Pointer to track the node before duplicates

    for head != nil {
        // Check for duplicates
        if head.Next != nil && head.Val == head.Next.Val {
            // Skip all nodes with the same value
            for head.Next != nil && head.Val == head.Next.Val {
                head = head.Next
            }
            // Remove duplicates by linking prev to head's next
            prev.Next = head.Next
        } else {
            // No duplicates, move prev forward
            prev = prev.Next
        }
        // Move head forward
        head = head.Next
    }

    return dummy.Next
}
```

