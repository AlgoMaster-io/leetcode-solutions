# [Leetcode 21: Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach](#iterative-approach)
   
### Recursive Approach

#### Intuition
This approach leverages the fact that the two lists are sorted. By comparing the heads of the lists, we can choose the smaller one to continue building our merged list recursively. This results in a simple and elegant solution that follows the definition of merging two lists step-by-step.

#### Implementation

```csharp
public ListNode MergeTwoLists(ListNode list1, ListNode list2)
{
    // Base case: if any of the list is null, return the other list
    if (list1 == null) return list2;
    if (list2 == null) return list1;

    // Compare the current nodes of list1 and list2
    if (list1.val < list2.val)
    {
        // If list1's value is smaller, continue merging from list1.next
        list1.next = MergeTwoLists(list1.next, list2);
        return list1;
    }
    else
    {
        // Otherwise, continue merging from list2.next
        list2.next = MergeTwoLists(list1, list2.next);
        return list2;
    }
}
```

#### Time Complexity
- **O(n + m)**: Where `n` and `m` are the lengths of `list1` and `list2`. Each node is processed exactly once.
#### Space Complexity
- **O(n + m)**: Recursive stack space could go up to `n + m` due to the depth of recursion.

---

### Iterative Approach

#### Intuition
Instead of using recursion, we use a loop to compare the nodes of the two lists step by step. We link the nodes to a new list sequentially, keeping track of the sorted order using a dummy node to simplify edge cases.

#### Implementation

```csharp
public ListNode MergeTwoListsIterative(ListNode list1, ListNode list2)
{
    // Create a dummy node to act as the starting point of the merged list
    ListNode dummy = new ListNode(-1);
    ListNode current = dummy;

    // Loop through both lists until one is depleted
    while (list1 != null && list2 != null)
    {
        if (list1.val < list2.val)
        {
            // Link the smaller node to the new list
            current.next = list1;
            list1 = list1.next;
        }
        else
        {
            // Link the smaller node to the new list
            current.next = list2;
            list2 = list2.next;
        }
        // Move the current pointer
        current = current.next;
    }

    // If one of the lists is not null, append the remainder
    if (list1 != null)
    {
        current.next = list1;
    }
    else
    {
        current.next = list2;
    }

    // Return the merged list, skip the dummy node
    return dummy.next;
}
```

#### Time Complexity
- **O(n + m)**: Similar to the recursive approach, each node in `list1` and `list2` is processed once.
#### Space Complexity
- **O(1)**: Iteratively merging does not utilize additional space beyond the pointers.

Each approach effectively merges two sorted lists into a single sorted list. The recursive solution is more intuitive but can lead to stack overflow with extremely large lists due to its space complexity. The iterative solution is more space-efficient and suitable for larger inputs.

