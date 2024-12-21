# [Leetcode 21: Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

## Approaches
1. [Iterative Approach](#iterative-approach)
2. [Recursive Approach](#recursive-approach)

### Iterative Approach

The idea for the iterative approach is to use a dummy node to form the resultant linked list. By maintaining a reference, or a pointer, to the head of the newly formed linked list, we can iterate through both input lists and compare their elements, appending the smaller element to the current node of the result list.

#### Intuition:
- Initiate a dummy node, which acts as a placeholder to build upon.
- Use a current pointer to keep track of where to add the next smallest element.
- Compare the nodes of both lists and append the smaller one to the current node.
- Move the pointer of the list whose node was appended.
- Once one list is exhausted, directly connect the remaining part of the other list.
- Return the next of our dummy node as it holds the head of the merged linked list.

```go
// Definition for singly-linked list.
type ListNode struct {
    Val int
    Next *ListNode
}

func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    // Create a dummy node to hold the result
    dummy := &ListNode{}
    // Current node to build the new list
    current := dummy

    // Iterate through both lists until one is empty
    for l1 != nil && l2 != nil {
        // Compare values of both lists
        if l1.Val < l2.Val {
            current.Next = l1     // Link to the smaller node
            l1 = l1.Next          // Move the pointer on list1
        } else {
            current.Next = l2     // Link to the smaller node
            l2 = l2.Next          // Move the pointer on list2
        }
        current = current.Next   // Move to newly added node
    }

    // Append the remainder of l1 or l2, whichever is left
    if l1 != nil {
        current.Next = l1
    } else {
        current.Next = l2
    }

    // The head of the merged list is next to the dummy node
    return dummy.Next
}
```

- **Time Complexity**: O(n + m) where n and m are the lengths of the two lists. Since every node is processed once.
- **Space Complexity**: O(1), we are reusing nodes and not using additional space.

### Recursive Approach

The recursive approach can be an elegant alternative. We use recursion to build the final linked list by always choosing the smaller head node and recursively merging the rest of the two lists.

#### Intuition:
- At each step of recursion, decide between l1 and l2 which node has the smaller value.
- Return the selected smaller node and recursively define its next node as the result of merging the remaining two lists.
- If either list is empty, return the other list as the rest of the merged list.

```go
func mergeTwoListsRecursive(l1 *ListNode, l2 *ListNode) *ListNode {
    // Base cases: If one list is empty, return the other
    if l1 == nil {
        return l2
    } else if l2 == nil {
        return l1
    }

    // Recursive step: Choose the smaller value and merge the rest recursively
    if l1.Val < l2.Val {
        l1.Next = mergeTwoListsRecursive(l1.Next, l2)
        return l1       // Return current node as part of the result
    } else {
        l2.Next = mergeTwoListsRecursive(l1, l2.Next)
        return l2       // Return current node as part of the result
    }
}
```

- **Time Complexity**: O(n + m) where n and m are the lengths of the two lists. Each node is visited once.
- **Space Complexity**: O(n + m), due to the recursion stack.

Both approaches effectively merge two sorted linked lists while preserving the ordering within a linear traversal time.

