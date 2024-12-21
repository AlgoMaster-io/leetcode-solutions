### [LeetCode Problem 82: Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

**Approaches:**
- [Approach 1: Two-Pointer Iterative Approach](#approach-1)
- [Approach 2: Dummy Node with Sentinel](#approach-2)
- [Approach 3: Recursive Approach](#approach-3)

---

### Approach 1: Two-Pointer Iterative Approach

#### Intuition:
To solve this problem, we can utilize a two-pointer technique. We will iterate through the list using a pointer, and maintain a previous pointer to manage the unique elements of the list. The idea is to identify the nodes with duplicate values and bypass them, effectively removing the duplicates from our final result.

1. **Initialize** a dummy node at the beginning of the list. This dummy node will help in managing edge cases.
2. **Iterate** through the list with a `current` pointer.
3. **Detect duplicates** by checking if `current` node's value is the same as `current.next` node's value.
4. **Skip duplicates** by moving the `previous` pointer's next to current's next non-duplicate node.
5. Only **move `previous` forward** if the sublist is clean of duplicates.
6. Return the modified list starting from the dummy node's next.

```csharp
public ListNode DeleteDuplicates(ListNode head) {
    // Dummy node initialization.
    ListNode dummy = new ListNode(0);
    dummy.next = head;

    ListNode previous = dummy; // 'previous' tracks the last non-duplicate node.

    while (head != null) {
        // If it's a start of duplicates sequence
        if (head.next != null && head.val == head.next.val) {
            // Skip all nodes with the same value.
            while (head.next != null && head.val == head.next.val) {
                head = head.next;
            }
            // Connect previous node to the node after duplicates.
            previous.next = head.next;
        } else {
            // Move 'previous' to node if no duplicate.
            previous = previous.next;
        }
        // Move forward.
        head = head.next;
    }

    return dummy.next; // Return head of modified list.
}
```

**Time Complexity:** O(n) because each element is visited only once.

**Space Complexity:** O(1) since we are reordering the list in place.

---

### Approach 2: Dummy Node with Sentinel

#### Intuition:
This approach is similar to the first, utilizing a dummy node, but it meticulously places a sentinel node before potential duplicates which helps in skipping over all duplicates in a more straightforward manner.

1. **Create a dummy head** for the given list to deal with edge cases such as list head being a duplicate.
2. Use a `current` pointer to traverse the list, connected to non-duplicate nodes.
3. Use a `runner` pointer to skip duplicates.
4. Whenever `runner` finds duplicates, `current` is updated to bypass them.

```csharp
public ListNode DeleteDuplicates(ListNode head) {
    // Use a sentinel node to handle edge cases easily.
    ListNode sentinel = new ListNode(0, head);

    ListNode current = sentinel;

    while (head != null) {
        // If it's a duplicate sublist.
        if (head.next != null && head.val == head.next.val) {
            // Skip all nodes with the duplicated value.
            while (head.next != null && head.val == head.next.val) {
                head = head.next;
            }
            // Hook current to the next set of distinct numbers.
            current.next = head.next;
        } else {
            // Move 'current' if no duplication sequence is found.
            current = current.next;
        }
        head = head.next; // Move forward in the list.
    }

    return sentinel.next; // Return new head.
}
```

**Time Complexity:** O(n)

**Space Complexity:** O(1)

---

### Approach 3: Recursive Approach

#### Intuition:
For the recursive solution, the strategy involves handling duplication after exploring down to the tail of the list, in a way that backtracks through the list eliminating duplicates in each recursive step.

1. Use a recursive function to process each node.
2. If duplicate sublist is detected, duplicate values are skipped and recursion is called on the next unique value.
3. Concatenate the results of recursive calls ensuring no duplicates.
4. Return cleaned list.

```csharp
public ListNode DeleteDuplicates(ListNode head) {
    if (head == null) return null;

    if (head.next != null && head.val == head.next.val) {
        // Skip all future duplicate nodes.
        while (head.next != null && head.val == head.next.val) {
            head = head.next;
        }
        // Recursively call to skip the current sequence of duplicates.
        return DeleteDuplicates(head.next);
    } else {
        // Process next node if it's not a duplicate and attach to result.
        head.next = DeleteDuplicates(head.next);
        return head;
    }
}
```

**Time Complexity:** O(n) due to visiting each node once.

**Space Complexity:** O(n) due to call stack usage during recursion.

---

