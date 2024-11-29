# [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approach 1: Two Passes

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        int length = 0;

        // Calculate the total length of the list
        ListNode* current = head;
        while (current != nullptr) {
            length++;
            current = current->next;
        }

        // Find the node before the one to remove
        int target = length - n;
        current = dummy;
        for (int i = 0; i < target; i++) {
            current = current->next;
        }

        // Remove the nth node
        current->next = current->next->next;

        return dummy->next;
    }
};
```

## Approach 2: One Pass with Two Pointers

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* first = dummy;
        ListNode* second = dummy;

        // Move the first pointer n + 1 steps ahead
        for (int i = 0; i <= n; i++) {
            first = first->next;
        }

        // Move both pointers until the first reaches the end
        while (first != nullptr) {
            first = first->next;
            second = second->next;
        }

        // Remove the nth node
        second->next = second->next->next;

        return dummy->next;
    }
};
```

