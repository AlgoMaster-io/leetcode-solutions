# [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approach: Iterative Solution with Dummy Node

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

type ListNode struct {
    Val  int
    Next *ListNode
}

func reverseKGroup(head *ListNode, k int) *ListNode {
    if head == nil || k == 1 {
        return head
    }

    dummy := &ListNode{Next: head}
    prevGroup := dummy

    for {
        // Find the k-th node from prevGroup
        kthNode := getKthNode(prevGroup, k)
        if kthNode == nil {
            break // Not enough nodes to reverse
        }

        nextGroup := kthNode.Next // Node after the k-th node
        prev := nextGroup         // Start reversal with nextGroup as tail
        current := prevGroup.Next

        // Reverse k nodes
        for current != nextGroup {
            temp := current.Next
            current.Next = prev
            prev = current
            current = temp
        }

        // Update the connections
        temp := prevGroup.Next
        prevGroup.Next = kthNode
        prevGroup = temp
    }

    return dummy.Next
}

func getKthNode(start *ListNode, k int) *ListNode {
    for start != nil && k > 0 {
        start = start.Next
        k--
    }
    return start
}
```

