# [LeetCode 82: Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## Approaches
- [Approach 1: Brute Force Approach using Dummy Node](#approach-1)
- [Approach 2: Two Pointer Approach](#approach-2)

## Approach 1: Brute Force using Dummy Node
### Intuition
The problem requires us to remove all nodes that have duplicate numbers from a sorted linked list, leaving only distinct numbers from the original list. A straightforward way is to use a dummy node that simplifies edge case handling, such as duplicates at the start of the list. We iterate through the list, checking each node for duplicates.

### Implementation
```cpp
#include <iostream>

// Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        // Create a dummy node to handle head deletions easily
        ListNode* dummy = new ListNode(0);
        dummy->next = head;

        // Pointer to track the node before current nodes being scanned
        ListNode* prev = dummy;

        // Traverse the list
        while (head != nullptr) {
            if (head->next != nullptr && head->val == head->next->val) {
                // Duplicate detected, skip all nodes with this value
                while (head->next != nullptr && head->val == head->next->val) {
                    head = head->next;
                }
                // Point the previous node to the node after the last duplicate
                prev->next = head->next;
            } else {
                // No duplicate, just move prev
                prev = prev->next;
            }
            // Move head to the next node
            head = head->next;
        }

        // Return the new head of the list
        return dummy->next;
    }
};
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of nodes in the list. We traverse each node once.
- **Space Complexity:** O(1), since we only use a few additional pointers.

## Approach 2: Two Pointer Approach
### Intuition
We use two pointers – `prev` and `curr`. The `prev` pointer is used to track the last node that is not a duplicate. The `curr` pointer traverses the list, skipping nodes with duplicate values.

### Implementation
```cpp
#include <iostream>

// Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        // Dummy node initialization for easier management
        ListNode* dummy = new ListNode(0, head);
        ListNode* prev = dummy; // Points to the last node in the finalized list

        while (head) {
            // Check if the current node is a start of a duplicate sequence
            if (head->next && head->val == head->next->val) {
                // Skip all duplicates
                while (head->next && head->val == head->next->val) {
                    head = head->next;
                }
                // Connect previous non-duplicate node to the node after duplicates
                prev->next = head->next;
            } else { 
                // Move prev to the current node if it's not a duplicate
                prev = prev->next;
            }
            // Move forward
            head = head->next;
        }

        return dummy->next;
    }
};
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of nodes in the linked list.
- **Space Complexity:** O(1), since modifications are made in-place.

