# [Leetcode 86: Partition List](https://leetcode.com/problems/partition-list/)

## Approaches
1. [Two Lists Approach](#two-lists-approach)
2. [In-place Rearrangement](#in-place-rearrangement)

### Two Lists Approach

The goal of the problem is to partition a linked list such that all nodes with values less than `x` come before nodes with values greater than or equal to `x`. The relative order of nodes within each subset must be preserved.

#### Intuition
- We can solve this problem by maintaining two separate lists, one for nodes less than `x` and another for nodes greater than or equal to `x`.
- We traverse the original list and add each node to either of the two lists based on its value.
- Finally, we combine the two lists.

This approach is straightforward and easy to implement using additional O(N) space where N is the number of nodes in the list.

#### Solution

```go
// Definition for singly-linked list.
type ListNode struct {
    Val int
    Next *ListNode
}

func partition(head *ListNode, x int) *ListNode {
    // Create two dummy nodes to help initialize two separate lists
    // for values less than x (less) and values greater than or equal to x (greater)
    lessHead, greaterHead := &ListNode{}, &ListNode{}
    less, greater := lessHead, greaterHead

    // Traverse the original list
    for head != nil {
        if head.Val < x {
            // Add to the 'less' list
            less.Next = head
            less = less.Next
        } else {
            // Add to the 'greater' list
            greater.Next = head
            greater = greater.Next
        }
        // Move to the next node in the original list
        head = head.Next
    }

    // Terminate the 'greater' list to avoid a cycle in the linked list
    greater.Next = nil
    // Link the 'less' list with 'greater' list
    less.Next = greaterHead.Next

    // Return the start of the merged list which is after the less dummy
    return lessHead.Next
}
```

**Time Complexity**: O(N) where N is the number of nodes in the input list. We make a single pass through the list.  
**Space Complexity**: O(1) apart from the space used to store the output, since the rearrangement is done in-place.

### In-Place Rearrangement

#### Intuition
- Instead of using two additional linked lists, we can perform the partition in-place.
- We'll use two pointers to keep track of the last seen node in each of the `< x` and `>= x` segments while traversing and modifying the original list.

This reduces space usage since no extra storage is used for elements, but it requires careful manipulation of pointers.

#### Solution

```go
func partition(head *ListNode, x int) *ListNode {
    // Placeholder for the new head of the modified list
    dummy := &ListNode{0, head}
    prev, cur := dummy, head

    var lastLess *ListNode // last node in the less-than-x segment
    // First pass to identify the last node of the "less than x" segment
    for cur != nil {
        if cur.Val < x {
            lastLess = cur
        }
        prev = cur
        cur = cur.Next
    }

    cur = dummy.Next
    prev = dummy

    if lastLess == nil {
        return head // all nodes >= x; no reordering needed
    }

    // Second pass to partition the list
    for cur != nil {
        nextNode := cur.Next
        if cur.Val < x && prev != lastLess {
            // Move the current node after lastLess
            prev.Next = cur.Next
            cur.Next = lastLess.Next
            lastLess.Next = cur
            lastLess = cur // Update lastLess to the newly inserted node
        } else {
            prev = cur
        }
        cur = nextNode
    }

    return dummy.Next
}
```

**Time Complexity**: O(N), since we traverse the list a couple of times to achieve our target partition.  
**Space Complexity**: O(1) as we are using a constant amount of extra space for reordering the list in place.

In conclusion, both approaches effectively partition the list as required, with the two-list approach being simpler to implement, while the in-place rearrangement optimizes space usage.

