# [Leetcode 82: Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## Approaches:
1. [Two Pointers with Dummy Node](#two-pointers-with-dummy-node)

---

### Two Pointers with Dummy Node

**Intuition:**

In this problem, we are dealing with a sorted linked list. The goal is to remove all nodes that have duplicate numbers, leaving only distinct numbers from the original list. 

- The sorted nature of the list allows us to detect duplicates by simply comparing adjacent nodes.
- To handle edge cases, like a head that needs to be removed because it's duplicated, we can use a dummy node. A dummy node is a temporary node that helps simplify edge case handling, such as head deletion.
- By using two pointers, `prev` and `current`, we can manage list traversal and modification efficiently.

**Approach:**

1. Initialize a dummy node that points to the head of the linked list.
2. Use a pointer `prev` to track the last node before the sequence of duplicates.
3. Use another pointer `current` to iterate through the list.
4. For each node, compare it with the next node to check if it's a start of a duplicate sequence.
5. When a series of duplicates is detected, `prev`’s next is linked to the node after the duplicates.
6. If no duplicates are found, simply move `prev` to `current`.
7. Continue this process until the end of the list.
8. Finally, return `dummy.next` because `dummy` was pointing to the beginning of the list structure we have built.

**Code:**

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}

public class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        // Dummy node to handle edge cases easily
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        // Prev points to the last unique node
        ListNode prev = dummy;
        
        // Current is used to traverse the list
        ListNode current = head;
        
        while (current != null) {
            // We enter this loop if we are looking at a duplicate
            while (current.next != null && current.val == current.next.val) {
                current = current.next;
            }
            
            // If prev.next is current, no duplicates found; move prev to the next
            if (prev.next == current) {
                prev = prev.next;
            } else {
                // If duplicates were found, skip them
                prev.next = current.next;
            }
            
            // Move current forward
            current = current.next;
        }
        
        // Return the head of the deduped list
        return dummy.next;
    }
}
```

**Time Complexity:**

- O(n) where n is the number of nodes in the list. We traverse the list once.

**Space Complexity:**

- O(1) because we only use a fixed amount of extra space (pointers).

