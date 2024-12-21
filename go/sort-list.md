# [Leetcode 148: Sort List](https://leetcode.com/problems/sort-list/)

## Approaches:
1. [Bubble Sort Approach](#bubble-sort-approach)
2. [Merge Sort Approach](#merge-sort-approach)

---

### Bubble Sort Approach

Intuition:
The bubble sort algorithm involves iterating over the list and repeatedly swapping adjacent elements if they are in the wrong order. This is repeated until the entire list is sorted. Although bubble sort is not optimal for sorting a linked list, it provides a stepping stone to understanding sorting in linked structures.

```go
package main

type ListNode struct {
    Val  int
    Next *ListNode
}

func bubbleSort(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }

    swapped := true
    for swapped {
        swapped = false
        current := head
        for current.Next != nil {
            // Compare two adjacent nodes
            if current.Val > current.Next.Val {
                // Swap values if they are in the wrong order
                current.Val, current.Next.Val = current.Next.Val, current.Val
                swapped = true
            }
            current = current.Next
        }
    }
    return head
}
```
Time Complexity: O(n^2)   
Space Complexity: O(1)   

### Merge Sort Approach

Intuition:
Merge sort is a divide-and-conquer algorithm. The algorithm divides the list into halves until the base case (a list with zero or one element) is reached, then merges the halves by sorted order. It is efficient in sorting linked lists because it does not require random access to elements like an array.

```go
package main

type ListNode struct {
    Val  int
    Next *ListNode
}

func sortList(head *ListNode) *ListNode {
    // If the list is empty or has one node, it's already sorted
    if head == nil || head.Next == nil {
        return head
    }

    // Split the list into two halves
    mid := getMiddle(head)
    left := head
    right := mid.Next
    mid.Next = nil

    // Recursively sort the two halves
    left = sortList(left)
    right = sortList(right)

    // Merge the sorted halves
    return mergeTwoLists(left, right)
}

func getMiddle(head *ListNode) *ListNode {
    slow, fast := head, head

    // Use the fast-slow pointer strategy to find the middle
    for fast.Next != nil && fast.Next.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }
    return slow
}

func mergeTwoLists(l1, l2 *ListNode) *ListNode {
    dummy := &ListNode{} // Temporary dummy node to form the new sorted list
    tail := dummy        // Tail to build the new list

    // Merge nodes based on their value
    for l1 != nil && l2 != nil {
        if l1.Val < l2.Val {
            tail.Next = l1
            l1 = l1.Next
        } else {
            tail.Next = l2
            l2 = l2.Next
        }
        tail = tail.Next
    }
    // Append any remaining nodes from l1 or l2
    if l1 != nil {
        tail.Next = l1
    } else {
        tail.Next = l2
    }

    return dummy.Next
}
```

Time Complexity: O(n log n)   
Space Complexity: O(log n) [Due to recursive stack usage] 

Merge Sort is the optimal approach for sorting a linked list due to its time complexity and the nature of linked list node manipulations, which do not require additional space for reordering.

