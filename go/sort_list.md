# [148. Sort List](https://leetcode.com/problems/sort-list/)

## Approach 1: Merge Sort (Top-Down)

### Solution
```go
// Time Complexity: O(n log n)
// Space Complexity: O(log n) (due to recursion stack)
type ListNode struct {
    Val  int
    Next *ListNode
}

func sortList(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head // Base case: single node or empty list
    }

    // Split the list into two halves
    mid := getMiddle(head)
    rightHead := mid.Next
    mid.Next = nil

    // Recursively sort each half
    left := sortList(head)
    right := sortList(rightHead)

    // Merge the sorted halves
    return merge(left, right)
}

func getMiddle(head *ListNode) *ListNode {
    slow, fast := head, head
    for fast.Next != nil && fast.Next.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }
    return slow
}

func merge(l1, l2 *ListNode) *ListNode {
    dummy := &ListNode{Val: -1}
    current := dummy

    for l1 != nil && l2 != nil {
        if l1.Val < l2.Val {
            current.Next = l1
            l1 = l1.Next
        } else {
            current.Next = l2
            l2 = l2.Next
        }
        current = current.Next
    }

    // Attach the remaining nodes
    if l1 != nil {
        current.Next = l1
    } else {
        current.Next = l2
    }

    return dummy.Next
}
```

## Approach 2: Merge Sort (Bottom-Up)

### Solution
```go
// Time Complexity: O(n log n)
// Space Complexity: O(1)
func sortList(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head // Base case: single node or empty list
    }

    // Get the length of the list
    length := 0
    current := head
    for current != nil {
        length++
        current = current.Next
    }

    dummy := &ListNode{Next: head}

    // Bottom-up merge sort
    for step := 1; step < length; step *= 2 {
        prev := dummy
        curr := dummy.Next

        for curr != nil {
            // Split the list into two halves of size `step`
            left := curr
            right := split(left, step)
            curr = split(right, step)

            // Merge the two halves and attach to the sorted list
            prev.Next = merge(left, right)

            // Move `prev` to the end of the merged segment
            for prev.Next != nil {
                prev = prev.Next
            }
        }
    }

    return dummy.Next
}

func split(head *ListNode, size int) *ListNode {
    for i := 1; head != nil && i < size; i++ {
        head = head.Next
    }

    if head == nil {
        return nil
    }

    next := head.Next
    head.Next = nil // Disconnect the segment
    return next
}

func merge(l1, l2 *ListNode) *ListNode {
    dummy := &ListNode{Val: -1}
    current := dummy

    for l1 != nil && l2 != nil {
        if l1.Val < l2.Val {
            current.Next = l1
            l1 = l1.Next
        } else {
            current.Next = l2
            l2 = l2.Next
        }
        current = current.Next
    }

    if l1 != nil {
        current.Next = l1
    } else {
        current.Next = l2
    }

    return dummy.Next
}
```

