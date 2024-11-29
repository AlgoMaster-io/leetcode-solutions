# [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

## Approach 1: Iterative

### Solution
```cpp
// Time Complexity: O(n + m), where n and m are the lengths of the two lists
// Space Complexity: O(1)
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode dummy(-1); // Dummy node to simplify edge cases
        ListNode* current = &dummy;

        while (list1 != nullptr && list2 != nullptr) {
            if (list1->val <= list2->val) {
                current->next = list1; // Append list1's current node
                list1 = list1->next; // Move list1 to the next node
            } else {
                current->next = list2; // Append list2's current node
                list2 = list2->next; // Move list2 to the next node
            }
            current = current->next; // Move the pointer forward
        }

        // Append the remaining nodes from either list
        if (list1 != nullptr) {
            current->next = list1;
        } else {
            current->next = list2;
        }

        return dummy.next; // Return the merged list starting at dummy.next
    }
};
```

## Approach 2: Recursive

### Solution
```cpp
// Time Complexity: O(n + m), where n and m are the lengths of the two lists
// Space Complexity: O(n + m) (due to recursion stack)
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if (list1 == nullptr) return list2; // If list1 is empty, return list2
        if (list2 == nullptr) return list1; // If list2 is empty, return list1

        // Merge the smaller node with the result of the recursive call
        if (list1->val <= list2->val) {
            list1->next = mergeTwoLists(list1->next, list2);
            return list1;
        } else {
            list2->next = mergeTwoLists(list1, list2->next);
            return list2;
        }
    }
};
```

