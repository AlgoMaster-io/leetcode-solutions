# [LeetCode 876: Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approaches:
- [Approach 1: Two Pass Solution](#approach-1-two-pass-solution)
- [Approach 2: Fast and Slow Pointer Solution (Optimal)](#approach-2-fast-and-slow-pointer-solution-optimal)

---

## Approach 1: Two Pass Solution

### Intuition:

The basic idea here is to first traverse the linked list to determine the total length of the linked list. Once we have the total length, we can easily calculate the index for the middle node, and traverse the linked list again to that middle index to find the middle node.

### Algorithm:

1. Start traversing from the head to count the total number of nodes.
2. Divide the total node count by 2 to get the index of the middle node. (For even nodes, choose the second middle node)
3. Traverse again up to this middle index and return the node at this position.

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        int count = 0;
        ListNode* curr = head;
        
        // First pass to count the total number of nodes
        while (curr != nullptr) {
            count++;
            curr = curr->next;
        }
        
        int middleIndex = count / 2; // Index of the middle node (integer division)
        curr = head;
        
        // Second pass to reach the middle node
        for (int i = 0; i < middleIndex; i++) {
            curr = curr->next;
        }
        
        return curr; // This is the middle node
    }
};
```

**Time Complexity**: O(n) - The linked list is traversed twice, each taking linear time.

**Space Complexity**: O(1) - No additional space is used, apart from temporary pointers.

---

## Approach 2: Fast and Slow Pointer Solution (Optimal)

### Intuition:

A more optimal approach exploits the concept of two pointers moving at different speeds. If we have one pointer that moves twice as fast as the other, when the fast pointer reaches the end of the list, the slow pointer will be at the middle of the list. This eliminates the need for a second traversal.

### Algorithm:

1. Initialize two pointers, `slow` and `fast`, both starting at the head.
2. Move `slow` one step at a time and `fast` two steps at a time.
3. When `fast` reaches the end (or the node before end for odd/even lengths), `slow` will be at the middle node.
4. Return the position of `slow`.

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        
        // Move slow by 1 step and fast by 2 steps
        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
        }
        
        // Slow pointer is now at the middle node
        return slow;
    }
};
```

**Time Complexity**: O(n) - The fast pointer makes the traversal once, reaching the end of the list.

**Space Complexity**: O(1) - Only a constant amount of extra space is used by the two pointers.

