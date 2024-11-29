# [148. Sort List](https://leetcode.com/problems/sort-list/)

## Approach 1: Merge Sort (Top-Down)

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(log n) (due to recursion stack)
#include <iostream>

struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};

class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head; // Base case: single node or empty list
        }

        // Split the list into two halves
        ListNode* mid = getMiddle(head);
        ListNode* rightHead = mid->next;
        mid->next = NULL;

        // Recursively sort each half
        ListNode* left = sortList(head);
        ListNode* right = sortList(rightHead);

        // Merge the sorted halves
        return merge(left, right);
    }

private:
    ListNode* getMiddle(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast->next != NULL && fast->next->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }

    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode dummy(-1);
        ListNode* current = &dummy;

        while (l1 != NULL && l2 != NULL) {
            if (l1->val < l2->val) {
                current->next = l1;
                l1 = l1->next;
            } else {
                current->next = l2;
                l2 = l2->next;
            }
            current = current->next;
        }

        // Attach the remaining nodes
        if (l1 != NULL) {
            current->next = l1;
        } else {
            current->next = l2;
        }

        return dummy.next;
    }
};
```

## Approach 2: Merge Sort (Bottom-Up)

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(1)
#include <iostream>

struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};

class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head; // Base case: single node or empty list
        }

        // Get the length of the list
        int length = 0;
        ListNode* current = head;
        while (current != NULL) {
            length++;
            current = current->next;
        }

        ListNode dummy(0);
        dummy.next = head;

        // Bottom-up merge sort
        for (int step = 1; step < length; step *= 2) {
            ListNode* prev = &dummy;
            ListNode* curr = dummy.next;

            while (curr != NULL) {
                // Split the list into two halves of size `step`
                ListNode* left = curr;
                ListNode* right = split(left, step);
                curr = split(right, step);

                // Merge the two halves and attach to the sorted list
                prev->next = merge(left, right);

                // Move `prev` to the end of the merged segment
                while (prev->next != NULL) {
                    prev = prev->next;
                }
            }
        }

        return dummy.next;
    }

private:
    ListNode* split(ListNode* head, int size) {
        for (int i = 1; head != NULL && i < size; i++) {
            head = head->next;
        }

        if (head == NULL) {
            return NULL;
        }

        ListNode* next = head->next;
        head->next = NULL; // Disconnect the segment
        return next;
    }

    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode dummy(-1);
        ListNode* current = &dummy;

        while (l1 != NULL && l2 != NULL) {
            if (l1->val < l2->val) {
                current->next = l1;
                l1 = l1->next;
            } else {
                current->next = l2;
                l2 = l2->next;
            }
            current = current->next;
        }

        if (l1 != NULL) {
            current->next = l1;
        } else {
            current->next = l2;
        }

        return dummy.next;
    }
};
```

