# [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

## Approach: Iterative Solution with Dummy Node

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
type ListNode struct {
    Val  int
    Next *ListNode
}

func reverseBetween(head *ListNode, left int, right int) *ListNode {
    if head == nil || left == right {
        return head
    }

    dummy := &ListNode{0, head} // Dummy node to simplify edge cases
    prev := dummy

    // Step 1: Move prev to the node before the "left" position
    for i := 1; i < left; i++ {
        prev = prev.Next
    }

    // Step 2: Reverse the sublist from "left" to "right"
    current := prev.Next // The first node to be reversed
    var next, prevSublist *ListNode // Temporary variables

    for i := 0; i <= right-left; i++ {
        next = current.Next // Store the next node
        current.Next = prevSublist // Reverse the current node
        prevSublist = current // Move prevSublist to the current node
        current = next // Move to the next node
    }

    // Step 3: Reconnect the reversed sublist with the rest of the list
    prev.Next.Next = current // Connect the tail of the reversed sublist
    prev.Next = prevSublist // Connect the head of the reversed sublist

    return dummy.Next // Return the updated list
}
```

