# [Leetcode 876: Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approaches
- [Approach 1: Counting the Number of Nodes](#approach-1-counting-the-number-of-nodes)
- [Approach 2: Two Pointers Technique](#approach-2-two-pointers-technique)

## Approach 1: Counting the Number of Nodes

### Intuition:
To find the middle of a linked list, one straightforward approach is to count the total number of nodes in the list first, and then iterate through the list again to find the node at the `n/2` position, where `n` is the total number of nodes. 

### Steps:
1. Traverse the linked list to count the total number of nodes.
2. Calculate the index of the middle node, which is `n/2` using integer division for even numbers (the middle is the second middle node).
3. Traverse the list again up to this middle index.
4. Return the middle node.

### Code:
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        // Step 1: Count the total number of nodes
        int count = 0;
        ListNode current = head;
        while (current != null) {
            count++;
            current = current.next;
        }
        
        // Step 2: Calculate the middle index
        int middleIndex = count / 2;
        
        // Step 3: Traverse again to the middle node
        current = head;
        for (int i = 0; i < middleIndex; i++) {
            current = current.next;
        }
        
        // Step 4: Return the middle node
        return current;
    }
}
```

### Complexity Analysis:
- **Time Complexity:** O(n), where n is the number of nodes in the list. We traverse the list twice.
- **Space Complexity:** O(1). We use a constant amount of space.

## Approach 2: Two Pointers Technique

### Intuition:
The two-pointer technique is often very efficient for linked list problems. We can use two pointers, `slow` and `fast`, to identify the middle of the list. The `fast` pointer moves twice as fast as the `slow` pointer. By the time the `fast` pointer reaches the end of the list, the `slow` pointer will be at the middle.

### Steps:
1. Initialize two pointers: `slow` and `fast`, both starting at the head of the list.
2. Move the `slow` pointer by one node and the `fast` pointer by two nodes in each iteration.
3. Continue moving the pointers until `fast` reaches the end of the list or the node just before the end (in case of even length).
4. The `slow` pointer will be at the middle of the list when the loop terminates.
5. Return the node at the `slow` pointer.

### Code:
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        // Initialize two pointers, 'slow' and 'fast'
        ListNode slow = head, fast = head;
        
        // Move 'slow' by 1 step and 'fast' by 2 steps
        while (fast != null && fast.next != null) {
            slow = slow.next; // slow moves 1 step
            fast = fast.next.next; // fast moves 2 steps
        }
        
        // When 'fast' reaches the end, 'slow' will be at the middle
        return slow;
    }
}
```

### Complexity Analysis:
- **Time Complexity:** O(n), where n is the number of nodes in the list. Each pointer traverses at most n elements.
- **Space Complexity:** O(1). We use only a fixed amount of space regardless of the input size. 

Both approaches help in understanding different strategies to deal with linked lists, while the second approach is generally preferred for its elegance and efficiency.

