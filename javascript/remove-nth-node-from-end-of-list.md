# [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approaches:
- [Approach 1: Brute Force Using Two Passes](#approach-1-brute-force-using-two-passes)
- [Approach 2: One Pass - Fast and Slow Pointer Technique](#approach-2-one-pass-fast-and-slow-pointer-technique)

### Approach 1: Brute Force Using Two Passes

**Intuition:**

The simplest way to solve this problem is to first determine the length of the linked list and then locate the node that needs to be removed. By traversing the list in two passes, we can remove the nth node from the end efficiently.

#### Steps:
1. Traverse the linked list to compute its total size.
2. Calculate the position from the start: `nth-from-end = length - n`.
3. Traverse the list again to reach this position and remove the node.

This approach is straightforward and operates in two distinct passes.

```javascript
function removeNthFromEnd(head, n) {
    // Step 1: Calculate the total length of the linked list
    let length = 0;
    let current = head;
    while (current !== null) {
        length++;
        current = current.next;
    }
    
    // Step 2: Calculate the target position from the start
    let target = length - n;
    
    // Use a dummy node to handle edge cases smoothly
    let dummy = new ListNode(0);
    dummy.next = head;
    current = dummy;
    
    // Step 3: Traverse the list to the target position
    while (target > 0) {
        current = current.next;
        target--;
    }
    
    // Step 4: Remove the node
    current.next = current.next.next;
    
    return dummy.next;
}
```

**Time Complexity:** O(L), where L is the length of the linked list.  
**Space Complexity:** O(1), as we only use a fixed amount of extra space.

### Approach 2: One Pass - Fast and Slow Pointer Technique

**Intuition:**

A more optimized method to solve this problem is to use two pointers. By maintaining a gap of 'n' nodes between two pointers, once the fast pointer reaches the end of the list, the slow pointer will be at the node just before the desired node to be removed.

#### Steps:
1. Initialize two pointers, `fast` and `slow`, both at the dummy node which is the start before head.
2. Move `fast` pointer `n + 1` steps ahead to maintain the gap.
3. Move both `fast` and `slow` one step at a time until `fast` reaches the end.
4. Now, `slow.next` will be the node to remove. Adjust the pointers to remove it.

This approach effectively finds and removes the nth node from the end in a single pass.

```javascript
function removeNthFromEnd(head, n) {
    // Create a dummy node to handle edge cases
    let dummy = new ListNode(0);
    dummy.next = head;

    // Step 1: Initialize two pointers at the start
    let fast = dummy;
    let slow = dummy;

    // Step 2: Move fast pointer n+1 steps to maintain a gap of n
    for (let i = 0; i <= n; i++) {
        fast = fast.next;
    }
    
    // Step 3: Move both pointers till fast reaches the end
    while (fast !== null) {
        fast = fast.next;
        slow = slow.next;
    }
    
    // Step 4: Remove the target node
    slow.next = slow.next.next;

    return dummy.next;
}
```

**Time Complexity:** O(L), where L is the length of the linked list.  
**Space Complexity:** O(1), as we only use a fixed amount of extra space.

Both approaches are effective, but the fast and slow pointer technique is more optimal as it requires only a single traversal of the linked list, making it a preferred solution for interview settings.

