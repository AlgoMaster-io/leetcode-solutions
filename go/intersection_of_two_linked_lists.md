# [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(m * n)
// Space Complexity: O(1)
type ListNode struct {
    Val  int
    Next *ListNode
}

func getIntersectionNode(headA, headB *ListNode) *ListNode {
    // Iterate through each node of list A
    for headA != nil {
        temp := headB

        // Check if it matches any node in list B
        for temp != nil {
            if headA == temp {
                return headA // Intersection found
            }
            temp = temp.Next
        }

        headA = headA.Next
    }

    return nil // No intersection
}
```

## Approach 2: Using HashSet (Intermediate)

### Solution
go
```go
// Time Complexity: O(m + n)
// Space Complexity: O(m) or O(n)
import "container/list"

type ListNode struct {
    Val  int
    Next *ListNode
}

func getIntersectionNode(headA, headB *ListNode) *ListNode {
    visitedNodes := make(map[*ListNode]bool)

    // Add all nodes of list A to the set
    for headA != nil {
        visitedNodes[headA] = true
        headA = headA.Next
    }

    // Check for intersection in list B
    for headB != nil {
        if visitedNodes[headB] {
            return headB // Intersection found
        }
        headB = headB.Next
    }

    return nil // No intersection
}
```

## Approach 3: Two Pointers (Optimal)

### Solution
go
```go
// Time Complexity: O(m + n)
// Space Complexity: O(1)
type ListNode struct {
    Val  int
    Next *ListNode
}

func getIntersectionNode(headA, headB *ListNode) *ListNode {
    if headA == nil || headB == nil {
        return nil
    }

    pointerA := headA
    pointerB := headB

    // Traverse both lists
    for pointerA != pointerB {
        // Switch to the other list when reaching the end
        if pointerA == nil {
            pointerA = headB
        } else {
            pointerA = pointerA.Next
        }

        if pointerB == nil {
            pointerB = headA
        } else {
            pointerB = pointerB.Next
        }
    }

    return pointerA // Either intersection node or nil
}
```

