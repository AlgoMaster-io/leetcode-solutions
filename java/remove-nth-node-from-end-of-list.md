# [Leetcode Problem 19: Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approaches
1. [Two Pass Algorithm](#two-pass-algorithm)
2. [One Pass Algorithm using Two Pointers](#one-pass-algorithm-using-two-pointers)

---

## Two Pass Algorithm

### Intuition
The basic idea behind this approach is to determine the length of the linked list and then remove the (L-n+1)'th node from the beginning (where L is the length of the list). In the first pass, we calculate the total length of the linked list. Then in the second pass, we traverse the list again up to the (L-n+1)'th node and remove it.

### Steps
1. Traverse the entire list to compute the total length `L`.
2. Find the node just before the (L-n+1)th node (this will be at position (L-n)) and change its next pointer to skip the nth node from the end.
3. Return the head of the modified list.

### Time Complexity
- **O(L)**: We perform two full traversals of the list, where L is the length of the list.

### Space Complexity
- **O(1)**: Only a constant amount of extra space is used.

### Java Solution
```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    // Create dummy node which further helps to handle edge cases such as removing the first node.
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    
    // Calculate the total length of the list
    int length = 0;
    ListNode current = head;
    while (current != null) {
        length++;
        current = current.next;
    }
    
    // Find the node preceding the one to be deleted
    length -= n;
    current = dummy;
    while (length > 0) {
        length--;
        current = current.next;
    }
    
    // Remove nth node from end
    current.next = current.next.next;
    
    return dummy.next;
}
```

---

## One Pass Algorithm using Two Pointers

### Intuition
We can improve our solution by using two pointers with a gap of n nodes between them. By moving both pointers simultaneously until the fast one reaches the end, the slow pointer will be just before the node we want to remove.

### Steps
1. Initialize two pointers: `fast` and `slow` both pointing to a dummy node ahead of head.
2. Move `fast` pointer `n+1` steps forward to maintain a gap of `n` between `slow` and `fast`.
3. Move both `fast` and `slow` pointers until `fast` pointer reaches the end.
4. The `slow` pointer will be at the node just before the target node. Adjust its next pointer to skip the nth node from the end.
5. Return the head of the modified list.

### Time Complexity
- **O(L)**: We make a single traversal of the list.

### Space Complexity
- **O(1)**: Only a constant amount of extra space is used.

### Java Solution
```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    // Initialize a dummy node which helps in handling edge cases like removing the head
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    
    // Initialize two pointers both at the dummy node
    ListNode slow = dummy;
    ListNode fast = dummy;
    
    // Move fast ahead by n+1 steps to create the required gap
    for (int i = 0; i <= n; i++) {
        fast = fast.next;
    }
    
    // Move both pointers till fast reaches the end of the list
    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }
    
    // Adjust the pointers to skip the nth node
    slow.next = slow.next.next;
    
    // Return the updated list starting from the node next to dummy
    return dummy.next;
}
```

Each of these solutions provides a method to reliably remove the nth node from the end of a singly linked list, with optimizations improving the efficiency of traversal.

