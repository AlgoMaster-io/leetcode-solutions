# [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approach 1: Using HashSet to Track Visited Nodes

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_set>

// Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        std::unordered_set<ListNode*> visited;

        while (head != nullptr) {
            // If the node has been visited before, it is the start of the cycle
            if (visited.find(head) != visited.end()) {
                return head;
            }
            visited.insert(head);
            head = head->next;
        }

        return nullptr; // No cycle detected
    }
};
```

## Approach 2: Floyd's Cycle Detection Algorithm (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)

// Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (head == nullptr || head->next == nullptr) {
            return nullptr; // No cycle possible
        }

        ListNode* slow = head;
        ListNode* fast = head;

        // Detect if a cycle exists
        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;

            if (slow == fast) { // Cycle detected
                ListNode* pointer = head;

                // Find the start of the cycle
                while (pointer != slow) {
                    pointer = pointer->next;
                    slow = slow->next;
                }

                return pointer; // Start of the cycle
            }
        }

        return nullptr; // No cycle detected
    }
};
```

