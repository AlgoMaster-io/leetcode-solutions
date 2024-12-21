# [Leetcode 92: Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

## Approaches:
1. [Iterative Traversal and Reversal](#iterative-traversal-and-reversal)
2. [In-place Reversal using a single pass](#in-place-reversal-using-a-single-pass)

### Iterative Traversal and Reversal

**Intuition:**

This approach involves multiple traversals of the linked list. We first traverse to reach the `m-th` node, and then reverse the sublist from `m-th` to `n-th` nodes. After reversal, reattach the reversed sublist to the original list. Although this approach is intuitive, it's less efficient than a single-pass approach.

**Algorithm:**

1. Traverse the linked list until reaching the node just before position `m` (let's call it `prev`).
2. Reverse the sublist from `m` to `n` using the classic linked list reversal technique.
3. Reattach the reversed sublist back to the main list by reconnecting `prev.next` to the new head of the reversed sublist and the last node of the sublist to `end.next`.

```go
// ListNode definition for singly-linked list.
type ListNode struct {
    Val int
    Next *ListNode
}

func reverseBetween(head *ListNode, left int, right int) *ListNode {
    if head == nil || left == right {
        return head
    }

    // Create a dummy node to simplify edge cases
    dummy := &ListNode{Next: head}
    prev := dummy

    // Move `prev` to the node just before the start of the reversal range
    for i := 0; i < left-1; i++ {
        prev = prev.Next
    }

    // `start` will be the first node of the reverse range
    start := prev.Next
    // `then` is the node we will reverse
    then := start.Next

    // Reverse the defined part of the linked list
    for i := 0; i < right-left; i++ {
        start.Next = then.Next // make `start` skip `then`
        then.Next = prev.Next // reverse the node
        prev.Next = then // connect `prev` to the newly reversed node
        then = start.Next // move forward with the sublist
    }

    return dummy.Next
}
```

**Time Complexity:** O(n), where n is the number of nodes in the linked list, as we may need to reverse almost all elements.

**Space Complexity:** O(1), since we are performing the operations in place.

### In-place Reversal using a single pass

**Intuition:**

Instead of multiple traversals, this approach performs a single pass through the list. We utilize a dummy node at the start to handle edge cases, and pointers to keep track of positions before, during, and after the reversal section.

**Algorithm:**

1. Initialize a dummy node that simplifies handling edge cases.
2. Locate the node before the reversal section.
3. Use two pointers (`start` and `then`) initialized within the sublist to facilitate the reverse.
4. Iteratively reverse the list section by section, updating pointers smartly as you go.

This approach streamlines the number of operations by maintaining pointers that efficiently manage the sub-list reversal, leading to faster execution.

```go
// ListNode definition for singly-linked list.
type ListNode struct {
    Val int
    Next *ListNode
}

func reverseBetween(head *ListNode, left int, right int) *ListNode {
    // Edge case checks
    if head == nil || left == right {
        return head
    }

    dummy := &ListNode{Next: head}
    prev := dummy

    // Traverse to the node right before where reversal begins
    for i := 0; i < left-1; i++ {
        prev = prev.Next
    }

    // Begin reversal
    start := prev.Next
    then := start.Next

    for i := 0; i < right-left; i++ {
        start.Next = then.Next
        then.Next = prev.Next
        prev.Next = then
        then = start.Next
    }

    return dummy.Next
}
```

**Time Complexity:** O(n), since we perform operations in a single pass through the nodes that are part of the linked list.

**Space Complexity:** O(1), as only a constant amount of extra space is used.

