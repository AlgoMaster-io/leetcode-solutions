[21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

### Approaches
1. [Iterative Merge Approach](#iterative-merge-approach)
2. [Recursive Merge Approach](#recursive-merge-approach)

---

### Iterative Merge Approach

#### Intuition
The easiest way to merge two sorted lists is to use an iterative approach. We can start by creating a dummy node that acts as a placeholder for the start of the merged list. We then use a current pointer to iterate through both lists and append the smaller node to the current node until we reach the end of one of the lists. Finally, we append any remaining nodes from the non-empty list.

#### Solution
```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    // Create a dummy node to act as the start of the merged list
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;

    // While neither list is empty, pick the smaller head and append it to the merged list
    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) {
            current.next = list1;
            list1 = list1.next;
        } else {
            current.next = list2;
            list2 = list2.next;
        }
        current = current.next;
    }

    // At this point, at least one of the two lists is null
    // Append the remaining non-null list to the merged list
    if (list1 != null) {
        current.next = list1;
    } else {
        current.next = list2;
    }

    // Return the merged list starting from the node after the dummy node
    return dummy.next;
}
```

#### Time Complexity
- **Time:** O(n + m) where n and m are the lengths of the input lists.
- **Space:** O(1) since no additional space proportional to input size is used.

---

### Recursive Merge Approach

#### Intuition
Alternatively, we can use a recursive solution to merge the two lists. The recursive approach involves merging the first nodes of each list, followed by the recursively merged result of the remainder of the lists. This is achieved by continually selecting the smaller head node between the two lists until all nodes have been merged.

#### Solution
```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    // Base cases: if one list is empty, return the other list
    if (list1 == null) {
        return list2;
    }
    if (list2 == null) {
        return list1;
    }

    // Recursive merging: select the smaller head and merge with the rest
    if (list1.val <= list2.val) {
        list1.next = mergeTwoLists(list1.next, list2);
        return list1;
    } else {
        list2.next = mergeTwoLists(list1, list2.next);
        return list2;
    }
}
```

#### Time Complexity
- **Time:** O(n + m) where n and m are the lengths of the input lists, as each node is only visited once.
- **Space:** O(n + m) due to the recursion stack, which holds a frame for each recursive call.

