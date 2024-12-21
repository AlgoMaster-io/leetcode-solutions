# [Leetcode 92: Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

## Approaches
1. [Basic Iterative Approach](#basic-iterative-approach)
2. [Optimized Iterative Approach](#optimized-iterative-approach)

### Basic Iterative Approach

**Intuition:**  
The problem requires us to reverse a specified portion of the linked list, from position `left` to `right`. We can solve this problem by locating the node at `left`, reversing nodes up to `right`, and then correctly connecting the non-reversed parts of the list.

1. Traverse the list until you reach the node before position `left`.
2. Reverse the sublist from `left` to `right`.
3. Reconnect the reversed sublist back to the list.

**Code:**

```csharp
public ListNode ReverseBetween(ListNode head, int left, int right) {
    // dummy node initialized to handle edge cases easily
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    
    // step 1: move previous node to the node before position left
    ListNode prev = dummy;
    for (int i = 0; i < left - 1; i++) {
        prev = prev.next;
    }
    
    // Reverse the sublist
    ListNode current = prev.next;
    ListNode next = null;
    ListNode prevSectionTail = prev;
    
    for (int i = 0; i < right - left + 1; i++) {
        next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }
    
    // Connecting the reversed part with the remaining sections
    prevSectionTail.next.next = current;
    prevSectionTail.next = prev;
    
    return dummy.next;
}
```

**Time Complexity:** O(n), where n is the number of nodes in the linked list.  
**Space Complexity:** O(1), as we are using a constant amount of additional memory.

### Optimized Iterative Approach

**Intuition:**  
This approach is similar to the basic iterative approach but with slight optimizations for readability and understanding.

1. We still use a dummy node to simplify edge cases.
2. Move `prev` to point to just before the `left` node.
3. Reverse the list from `left` to `right` directly while traversing, using three pointers.
4. Reconnect the `left`-`right` sublist to the main linked list.

**Code:**

```csharp
public ListNode ReverseBetween(ListNode head, int left, int right) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    
    ListNode pre = dummy;
    for (int i = 0; i < left - 1; i++) {
        pre = pre.next;
    }

    ListNode current = pre.next;
    ListNode tmp = null;
    ListNode tail = current;
    
    for (int i = 0; i < right - left + 1; i++) {
        ListNode next = current.next;
        current.next = tmp;
        tmp = current;
        current = next;
    }
    
    pre.next = tmp;
    tail.next = current;
    
    return dummy.next;
}
```

**Time Complexity:** O(n), where n is the number of nodes in the linked list.  
**Space Complexity:** O(1), as this approach only uses a fixed amount of additional space.

By solving the problem with these methods, we ensure efficient traversal and modification of the linked list while maintaining clean and understandable code.

