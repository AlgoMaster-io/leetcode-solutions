# [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

## Approach 1: Iterative

### Solution

```go
// Time Complexity: O(n + m), where n and m are the lengths of the two lists
// Space Complexity: O(1)
package main

// Definition for singly-linked list.
type ListNode struct {
    Val  int
    Next *ListNode
}

func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    dummy := &ListNode{-1, nil} // Dummy node to simplify edge cases
    current := dummy

    for list1 != nil && list2 != nil {
        if list1.Val <= list2.Val {
            current.Next = list1 // Append list1's current node
            list1 = list1.Next // Move list1 to the next node
        } else {
            current.Next = list2 // Append list2's current node
            list2 = list2.Next // Move list2 to the next node
        }
        current = current.Next // Move the pointer forward
    }

    // Append the remaining nodes from either list
    if list1 != nil {
        current.Next = list1
    } else {
        current.Next = list2
    }

    return dummy.Next // Return the merged list starting at dummy.Next
}
```

## Approach 2: Recursive

### Solution

```go
// Time Complexity: O(n + m), where n and m are the lengths of the two lists
// Space Complexity: O(n + m) (due to recursion stack)
package main

// Definition for singly-linked list.
type ListNode struct {
    Val  int
    Next *ListNode
}

func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    if list1 == nil {
        return list2 // If list1 is empty, return list2
    }
    if list2 == nil {
        return list1 // If list2 is empty, return list1
    }

    // Merge the smaller node with the result of the recursive call
    if list1.Val <= list2.Val {
        list1.Next = mergeTwoLists(list1.Next, list2)
        return list1
    } else {
        list2.Next = mergeTwoLists(list1, list2.Next)
        return list2
    }
}
```

