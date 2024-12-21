# [Leetcode 19: Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approaches
- [Approach 1: Two Pass Algorithm](#approach-1-two-pass-algorithm)
- [Approach 2: Single Pass Using Two Pointers](#approach-2-single-pass-using-two-pointers)

### Approach 1: Two Pass Algorithm

#### Intuition
The two-pass method involves first calculating the length of the linked list, and then identifying the node from the start that corresponds to the nth node from the end using this length.

#### Steps:
1. Traverse the linked list to calculate its length.
2. Compute the position of the node to remove from the beginning: `(length - n + 1)`.
3. Traverse the list again to the node one position before the target node and adjust the pointers to skip the target node.

#### Code

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummy = new ListNode(0, head);
        ListNode *current = head;
        int length = 0;

        // First pass to calculate the length
        while (current != nullptr) {
            length++;
            current = current->next;
        }

        length -= n;
        current = dummy;

        // Find the node before the one we want to remove
        while (length > 0) {
            length--;
            current = current->next;
        }

        // Remove the nth node from the end
        current->next = current->next->next;

        // Return the head of modified list
        return dummy->next;
    }
};
```

#### Time Complexity
- **Time complexity:** O(L), where L is the length of the linked list. The list is traversed twice.
- **Space complexity:** O(1), no additional space is used.

### Approach 2: Single Pass Using Two Pointers

#### Intuition
Use a two-pointer technique to locate the nth node from the end in a single traversal. The idea is to maintain a gap of `n` nodes between two pointers. When the fast pointer reaches the end of the list, the slow pointer will be at the node just before the target.

#### Steps:
1. Initialize two pointers, `fast` and `slow`. Both start from a dummy node that precedes the head.
2. Move `fast` `n + 1` steps ahead. This sets up a gap of `n` nodes between `slow` and `fast`.
3. Move both pointers one step at a time until `fast` reaches the end of the list.
4. `slow.next` is the node to be deleted, adjust the `next` pointers to skip it.

#### Code

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummy = new ListNode(0, head);
        ListNode *fast = dummy;
        ListNode *slow = dummy;

        // Move fast ahead by n+1 steps
        for (int i = 0; i <= n; i++) {
            fast = fast->next;
        }

        // Move both pointers until fast reaches the end
        while (fast != nullptr) {
            fast = fast->next;
            slow = slow->next;
        }

        // Skip the target node
        slow->next = slow->next->next;

        // Return the head of modified list
        return dummy->next;
    }
};
```

#### Time Complexity
- **Time complexity:** O(L), where L is the length of the linked list. The list is traversed once.
- **Space complexity:** O(1), no additional space beyond variables is used.

