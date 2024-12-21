# [Leetcode 206: Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

## Table of Contents
- [Approach 1: Iterative Solution](#approach-1-iterative-solution)
- [Approach 2: Recursive Solution](#approach-2-recursive-solution)

## Approach 1: Iterative Solution

### Intuition
The iterative approach involves using a loop to reverse the pointers of the linked list's nodes one by one. By starting from the head, we can keep track of the previous node and progressively change the current node's next pointer to point to this previous node. We move through the list, effectively flipping all the pointers, until the entire list is reversed.

### Algorithm
1. Initialize three pointers: `prev` (set to `null`), `curr` (set to `head`), and `next` (to store the `curr.next` node temporarily).
2. Iterate through the list:
   - Store `curr.next` in `next` to save the rest of the list.
   - Reverse the pointer of the `curr` node to point to `prev`.
   - Move `prev` and `curr` one step forward.
3. Continue until `curr` becomes `null`.
4. The new head of the list is the `prev` node.

### Code

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null; // Previous node, initially null
    ListNode curr = head; // Current node starts from the head
    while (curr != null) {
        ListNode next = curr.next; // Store next node
        curr.next = prev; // Reverse the current node's pointer
        prev = curr; // Move prev to current
        curr = next; // Move curr to next
    }
    return prev; // New head of the reversed list
}
```

### Complexity Analysis
- **Time complexity**: O(n) – We are iterating through the list once, where `n` is the number of elements in the list.
- **Space complexity**: O(1) – We use only a constant amount of extra space.

## Approach 2: Recursive Solution

### Intuition
The recursive approach involves diving deeper into the list until we reach the end and starting the reversal from there. As each recursive call returns, we reverse the pointers at that level. This way, the pointers are adjusted in a postfix manner.

### Algorithm
1. Base Case: If `head` is `null` or `head.next` is `null`, return `head`. This means we found the new head of the reversed list.
2. Recursive Case: Reverse the rest of the list and get the new head.
3. Once the rest of the list is reversed, connect `head.next.next` to `head` and set `head.next` to `null`.
4. Return the new head obtained from the recursive call.

### Code

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head; // Base case: if list is empty or has one node
    }
    ListNode newHead = reverseList(head.next); // Recurse for the rest of the list
    head.next.next = head; // Reverse the current node's pointer
    head.next = null; // Set the current node's next to null
    return newHead; // Return new head result from recursion
}
```

### Complexity Analysis
- **Time complexity**: O(n) – Each node's operation is constant, and we perform it for every node.
- **Space complexity**: O(n) – The recursion stack will have `n` frames, one for each node in the list.

