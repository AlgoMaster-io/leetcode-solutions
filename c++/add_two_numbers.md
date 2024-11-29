# [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Approach: Iterative Solution with Carry

### Solution
```cpp
// Time Complexity: O(max(m, n)), where m and n are the lengths of the two lists
// Space Complexity: O(max(m, n)), for the new linked list

struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(0); // Dummy node to simplify the process
        ListNode* current = dummy; // Pointer to the current node in the result list
        int carry = 0; // Store carry from the addition

        // Traverse both lists
        while (l1 != nullptr || l2 != nullptr || carry != 0) {
            int sum = carry; // Start with the carry

            // Add values from l1 and l2 if they exist
            if (l1 != nullptr) {
                sum += l1->val;
                l1 = l1->next;
            }
            if (l2 != nullptr) {
                sum += l2->val;
                l2 = l2->next;
            }

            // Calculate new carry and the digit to store
            carry = sum / 10;
            current->next = new ListNode(sum % 10); // Store the last digit of the sum
            current = current->next; // Move to the next node
        }

        return dummy->next; // Skip the dummy node
    }
};
```

