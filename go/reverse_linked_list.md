# [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

## Approach 1: Iterative Solution

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)

type ListNode struct {
    Val  int
    Next *ListNode
}

func reverseList(head *ListNode) *ListNode {
    var prev *ListNode = nil // Previous node, initially nil
    current := head           // Current node to process

    for current != nil {
        nextTemp := current.Next // Store the next node
        current.Next = prev      // Reverse the link
        prev = current           // Move prev to the current node
        current = nextTemp       // Move to the next node
    }

    return prev // New head of the reversed list
}
```

## Approach 2: Recursive Solution

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n) (due to recursion stack)

func reverseList(head *ListNode) *ListNode {
    // Base case: if the list is empty or has only one node
    if head == nil || head.Next == nil {
        return head
    }

    // Reverse the rest of the list
    reversedList := reverseList(head.Next)

    // Reverse the current node
    head.Next.Next = head
    head.Next = nil

    return reversedList // Return the new head
}
```

