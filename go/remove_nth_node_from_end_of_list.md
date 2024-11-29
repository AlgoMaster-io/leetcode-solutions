# [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approach 1: Two Passes

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
type ListNode struct {
    Val  int
    Next *ListNode
}

func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummy := &ListNode{Next: head}
    length := 0

    // Calculate the total length of the list
    current := head
    for current != nil {
        length++
        current = current.Next
    }

    // Find the node before the one to remove
    target := length - n
    current = dummy
    for i := 0; i < target; i++ {
        current = current.Next
    }

    // Remove the nth node
    current.Next = current.Next.Next

    return dummy.Next
}
```

## Approach 2: One Pass with Two Pointers

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummy := &ListNode{Next: head}
    first := dummy
    second := dummy

    // Move the first pointer n + 1 steps ahead
    for i := 0; i <= n; i++ {
        first = first.Next
    }

    // Move both pointers until the first reaches the end
    while first != nil {
        first = first.Next
        second = second.Next
    }

    // Remove the nth node
    second.Next = second.Next.Next

    return dummy.Next
}
```

