# LeetCode Problem: [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

## Approaches:
1. [Basic Approach: Iterative Reversal with Extra Space](#basic-approach-iterative-reversal-with-extra-space)
2. [Optimal Approach: Iterative In-Place Reversal](#optimal-approach-iterative-in-place-reversal)

---

### Basic Approach: Iterative Reversal with Extra Space

**Intuition:**
The idea is to split the linked list into three parts: 
- The sublist before the m-th node.
- The sublist from m-th to n-th node.
- The sublist after the n-th node.

We'll store the sublist from m-th to n-th into an auxiliary data structure, reverse it there, and then reconstruct the desired linked list.

```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        if (!head || left == right) return head;

        ListNode* dummy = new ListNode(-1); // To simplify edge cases where the head is reversible
        dummy->next = head;
        ListNode *prev = dummy;
        
        // Move `prev` to the node just before the `left` position
        for (int i = 1; i < left; ++i) {
            prev = prev->next;
        }
        
        ListNode *current = prev->next; // Current now points to `left` position node
        std::vector<ListNode*> nodes; // Store nodes between left and right
        
        // Collect nodes from position `left` to `right`
        for (int i = left; i <= right; ++i) {
            nodes.push_back(current);
            current = current->next;
        }
        
        // Reverse the collected nodes
        for (int i = nodes.size() - 1; i > 0; --i) {
            nodes[i]->next = nodes[i - 1];
        }
        
        // Handle connections to the rest of the list
        prev->next = nodes.back(); // Start of reversed sublist
        nodes.front()->next = current; // `current` is the node after `right`
        
        return dummy->next;
    }
};
```

**Time Complexity:** O(N), where N is the number of nodes from left to right.

**Space Complexity:** O(N), for storing nodes in the vector.

---

### Optimal Approach: Iterative In-Place Reversal

**Intuition:**
Instead of using extra space, we can reverse the linked list from position m to n in-place. The technique involves manipulating pointers and reversing the specified segment directly.

1. Traverse to the node just before the starting point of reversal.
2. Use a sublist reversal technique to reverse between `m` and `n` in one pass.

```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        if (!head || left == right) return head;

        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        ListNode* prev = dummy;
        
        // Move `prev` to the node just before the `left` position
        for (int i = 1; i < left; ++i) {
            prev = prev->next;
        }
        
        // Reversal process
        ListNode* current = prev->next;
        ListNode* nextNode = current->next;
        
        for (int i = 0; i < right - left; ++i) {
            current->next = nextNode->next;
            nextNode->next = prev->next;
            prev->next = nextNode;
            nextNode = current->next;
        }
        
        return dummy->next;
    }
};
```

**Time Complexity:** O(N), where N is the length of the list since we traverse the linked list once.

**Space Complexity:** O(1), as the reversal is done in place with constant extra space.

---

These approaches tackle the problem with varying space efficiencies, with the second one providing an optimal solution in terms of both time and space complexities.

