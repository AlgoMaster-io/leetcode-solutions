# [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approach 1: Using HashSet to Track Visited Nodes

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

type ListNode struct {
    Val  int
    Next *ListNode
}

func detectCycle(head *ListNode) *ListNode {
    visited := make(map[*ListNode]bool)

    for head != nil {
        // If the node has been visited before, it is the start of the cycle
        if visited[head] {
            return head
        }
        visited[head] = true
        head = head.Next
    }

    return nil // No cycle detected
}
```

## Approach 2: Floyd's Cycle Detection Algorithm (Optimal)

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

type ListNode struct {
    Val  int
    Next *ListNode
}

func detectCycle(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return nil // No cycle possible
    }

    slow, fast := head, head

    // Detect if a cycle exists
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next

        if slow == fast { // Cycle detected
            pointer := head

            // Find the start of the cycle
            while pointer != slow {
                pointer = pointer.Next
                slow = slow.Next
            }

            return pointer // Start of the cycle
        }
    }

    return nil // No cycle detected
}
```

