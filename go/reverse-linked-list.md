# [Leetcode 206: Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

## Approaches
1. [Iterative Approach](#iterative-approach)
2. [Recursive Approach](#recursive-approach)

## Iterative Approach

### Intuition
The iterative approach to reversing a linked list involves traversing the list only once, changing each node's next pointer to point to its previous node. We typically use three pointers to achieve this: `prev`, `curr`, and `next`. The `next` pointer helps to temporarily store the next node before we change `curr`'s next to `prev`, and `prev` is used to keep track of the reversed part of the list.

### Steps
1. Initialize three pointers: `prev` as `nil`, `curr` as `head`, and `next` as `nil`.
2. Traverse the list:
   - Save the next node: `next = curr.Next`.
   - Reverse the current node's pointer: `curr.Next = prev`.
   - Move `prev` and `curr` one step forward: `prev = curr` and `curr = next`.
3. At the end of the loop, `prev` will be the new head of the reversed list.

### Time Complexity
- **O(n)**: We traverse the list once where `n` is the number of nodes in the list.

### Space Complexity
- **O(1)**: We use a constant amount of space for pointers.

```go
// Definition for singly-linked list.
type ListNode struct {
    Val int
    Next *ListNode
}

func reverseList(head *ListNode) *ListNode {
    var prev *ListNode = nil
    curr := head

    for curr != nil {
        // Store next node
        next := curr.Next
        // Reverse the current node's pointer
        curr.Next = prev
        // Move pointers one position forward
        prev = curr
        curr = next
    }
    return prev
}
```

## Recursive Approach

### Intuition
The recursive approach is more elegant but slightly harder to get right. The idea is to reverse the rest of the list from the second node, then fix the current node's next pointer. We use the call stack to keep track of the previous nodes.

### Steps
1. Base case: if the list is empty or has only one node, return `head`.
2. Recursively reverse the rest of the list starting from `head.Next`.
3. After reversing, `head.Next` node's next should point to `head`.
4. Set `head.Next = nil` to mark the new end of the list.
5. Return the new head from the recursion.

### Time Complexity
- **O(n)**: We process each node once recursively.

### Space Complexity
- **O(n)**: Due to the recursion stack's size for `n` function calls.

```go
func reverseList(head *ListNode) *ListNode {
    // Base case: if head is null or there's only one node
    if head == nil || head.Next == nil {
        return head
    }

    // Recursively reverse the rest of the list
    rest := reverseList(head.Next)

    // Head becomes the tail of the reversed list
    head.Next.Next = head  // New link
    head.Next = nil        // Break the old link

    // rest is now the new head of the reversed list
    return rest
}
```

These approaches provide both iterative and recursive solutions to reverse a linked list efficiently, with detailed explanations aimed to shed light on the inner workings of each method.

