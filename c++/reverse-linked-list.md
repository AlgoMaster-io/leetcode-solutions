# [Reverse Linked List - Leetcode 206](https://leetcode.com/problems/reverse-linked-list/)

## Approaches
1. [Iterative Approach](#iterative-approach)
2. [Recursive Approach](#recursive-approach)

### Iterative Approach

The iterative approach is the most straightforward way to reverse a linked list. The intuition here is to traverse the list while adjusting the pointers to point in the opposite direction.

- **Intuition**: We maintain three pointers: `prev`, `curr`, and `next`. We initialize `prev` as `nullptr` and `curr` as the head of the linked list. In each iteration, we store the next node (using `curr->next`) in `next` to keep track of the remaining list. We then reverse the current node's `next` pointer to point to `prev` effectively reversing the node. Move `prev` and `curr` one step forward along the list until `curr` becomes `nullptr`.

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;  // Initialize previous pointer as nullptr
        ListNode* curr = head;     // Start from the head of the list
        
        while (curr) {  // Traverse the list
            ListNode* next = curr->next; // Store the next node
            curr->next = prev;           // Reverse the current node's pointer
            prev = curr;                 // Move prev one step forward
            curr = next;                 // Move curr one step forward
        }
        
        return prev;  // Prev will be the new head of the reversed list
    }
};
```

- **Time Complexity**: O(n), where n is the number of nodes in the linked list.
- **Space Complexity**: O(1), since we only use a fixed amount of extra space.

---

### Recursive Approach

This approach uses the call stack to reverse the linked list. The approach is elegant though less intuitive for those not familiar with recursion.

- **Intuition**: Consider the linked list as two parts: the head node and the rest of the list. If we can recursively reverse the rest of the list, then we just need to append the head node at the end of the reversed list. We recursively call reverseList on the `head->next`, once the recursion reaches the end of the list, the head of the reversed list will be returned.

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        // Base case: if head is null or there's only one element, it's reversed in itself
        if (!head || !head->next) {
            return head;
        }
        // Recurse on the rest of the list
        ListNode* reversedHead = reverseList(head->next);
        
        // Reverse the next node's pointer to point back to the head
        head->next->next = head;
        head->next = nullptr;  // Set the current node's next to null, as it will be the new tail
        
        return reversedHead; // Return the new head of the reversed list
    }
};
```

- **Time Complexity**: O(n), where n is the number of nodes in the linked list.
- **Space Complexity**: O(n), due to the recursion call stack.

This set of solutions provides a comprehensive understanding of how to reverse a linked list using both iterative and recursive techniques, each with its own trade-offs in terms of space and ease of understanding.

