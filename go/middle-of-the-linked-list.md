## [Leetcode 876: Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

### Approaches
1. [Brute Force: Two Pass Approach](#two-pass-approach)
2. [Optimized: Fast and Slow Pointer Technique](#fast-and-slow-pointer-technique)

---

### 1. Brute Force: Two Pass Approach

**Intuition:**

The idea is quite straightforward. We can solve this problem using two passes through the linked list:

- **First Pass:** Count the total number of nodes in the linked list.
- **Second Pass:** Use the count from the first pass to identify and return the middle node.

This method is simple to implement, but involves traversing the linked list twice, which can be optimized.

**Go Code with Comments:**

```go
// ListNode definition for singly-linked list.
type ListNode struct {
    Val  int
    Next *ListNode
}

func middleNode(head *ListNode) *ListNode {
    // First pass to count the total number of nodes.
    count := 0
    current := head
    for current != nil {
        count++
        current = current.Next
    }
    
    // Calculate the index of the middle node.
    middleIndex := count / 2
    
    // Second pass to reach the middle node.
    current = head
    for i := 0; i < middleIndex; i++ {
        current = current.Next
    }
    
    // Return the middle node.
    return current
}
```

**Time Complexity:** O(N) - where N is the number of nodes in the linked list. We traverse the list twice.

**Space Complexity:** O(1) - Constant space is used.

---

### 2. Optimized: Fast and Slow Pointer Technique

**Intuition:**

The fast and slow pointer technique allows us to find the middle node in a single traversal of the linked list:

- We maintain two pointers, `slow` and `fast`.
- `Slow` moves one step at a time, while `fast` moves two steps at a time.
- By the time `fast` reaches the end of the list, `slow` will be at the middle node.

This technique is efficient because it completes the task in one traversal.

**Go Code with Comments:**

```go
// ListNode definition for singly-linked list.
type ListNode struct {
    Val  int
    Next *ListNode
}

func middleNode(head *ListNode) *ListNode {
    // Initialize slow and fast pointers at the head of the linked list.
    slow := head
    fast := head
    
    // Traverse the list with fast moving two steps and slow moving one step.
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }
    
    // When fast reaches the end, slow will be at the middle node.
    return slow
}
```

**Time Complexity:** O(N) - We traverse the list once.

**Space Complexity:** O(1) - Only a constant amount of extra space is used.

In conclusion, while the two-pass approach is simple, the fast and slow pointer technique is more efficient. It finds the middle of the linked list in a single traversal, making it both time and space optimal for this problem.

