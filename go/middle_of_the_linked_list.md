# [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approach 1: Two Passes (Count Nodes)

### Solution
Go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)

type ListNode struct {
    Val  int
    Next *ListNode
}

func middleNode(head *ListNode) *ListNode {
    count := 0
    current := head

    // Count the total number of nodes
    for current != nil {
        count++
        current = current.Next
    }

    // Find the middle index
    middleIndex := count / 2
    current = head

    // Move to the middle node
    for i := 0; i < middleIndex; i++ {
        current = current.Next
    }

    return current
}
```

## Approach 2: Fast and Slow Pointers (Optimal)

### Solution
Go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)

type ListNode struct {
    Val  int
    Next *ListNode
}

func middleNode(head *ListNode) *ListNode {
    slow := head
    fast := head

    // Move fast pointer twice as fast as the slow pointer
    for fast != nil && fast.Next != nil {
        slow = slow.Next        // Move slow pointer one step
        fast = fast.Next.Next   // Move fast pointer two steps
    }

    return slow // Slow pointer will be at the middle
}
```

