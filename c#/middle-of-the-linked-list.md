# [Leetcode 876: Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approaches
- [Approach 1: Two-Pass Traversal](#approach-1-two-pass-traversal)
- [Approach 2: Fast and Slow Pointer (Optimal)](#approach-2-fast-and-slow-pointer-optimal)

---

## Approach 1: Two-Pass Traversal

### Intuition
The first approach to find the middle of the linked list is straightforward. We can traverse the linked list to count the total number of nodes. Then, we traverse the list again to find the middle node. This requires two passes over the linked list.

### Steps
1. Traverse the entire linked list to count the number of nodes, `n`.
2. Calculate the index of the middle node, which is `n / 2`.
3. Traverse the list again, stopping when you reach the middle node, and return it.

### Code
```csharp
public ListNode MiddleNode(ListNode head) {
    // First pass to count the total number of nodes
    int count = 0;
    ListNode current = head;
    while (current != null) {
        count++;
        current = current.next;
    }
    
    // Find the index of the middle node
    int middleIndex = count / 2;
    
    // Second pass to reach the middle node
    current = head;
    for (int i = 0; i < middleIndex; i++) {
        current = current.next;
    }
    
    // Return the middle node
    return current;
}
```

### Time Complexity
- **O(N):** We traverse the linked list twice, where N is the number of nodes.

### Space Complexity
- **O(1):** No additional space is used; only a few variables are employed.

---

## Approach 2: Fast and Slow Pointer (Optimal)

### Intuition
The optimal solution employs the two-pointer technique with a fast and a slow pointer. The slow pointer moves one step at a time, while the fast pointer moves two steps. When the fast pointer reaches the end of the list, the slow pointer will be at the middle. This approach only requires one pass through the list.

### Steps
1. Initialize two pointers, `slow` and `fast`, both starting at the head of the list.
2. Move the `slow` pointer one step at a time and the `fast` pointer two steps at a time.
3. When the `fast` pointer reaches the end, the `slow` pointer will be at the middle node.

### Code
```csharp
public ListNode MiddleNode(ListNode head) {
    // Initialize two pointers both set at the head of the list
    ListNode slow = head;
    ListNode fast = head;
    
    // Loop through the list until fast reaches the end
    while (fast != null && fast.next != null) {
        slow = slow.next;         // Move slow one step
        fast = fast.next.next;    // Move fast two steps
    }
    
    // Once the loop ends, slow will be at the middle node
    return slow;
}
```

### Time Complexity
- **O(N):** We traverse the linked list once with the pointers.

### Space Complexity
- **O(1):** We use constant extra space for the pointers.

---

These two methods offer different levels of efficiency. The second, utilizing the two-pointer technique, provides the most efficient solution.

