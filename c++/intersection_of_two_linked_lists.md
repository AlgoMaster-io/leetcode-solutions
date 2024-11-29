# 160. [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(m * n)
// Space Complexity: O(1)
class Solution {
public:
    ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {
        // Iterate through each node of list A
        while (headA != nullptr) {
            ListNode* temp = headB;

            // Check if it matches any node in list B
            while (temp != nullptr) {
                if (headA == temp) {
                    return headA; // Intersection found
                }
                temp = temp->next;
            }

            headA = headA->next;
        }

        return nullptr; // No intersection
    }
};
```

## Approach 2: Using HashSet (Intermediate)

### Solution
```cpp
// Time Complexity: O(m + n)
// Space Complexity: O(m) or O(n)
#include <unordered_set>

class Solution {
public:
    ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {
        std::unordered_set<ListNode*> visitedNodes;

        // Add all nodes of list A to the set
        while (headA != nullptr) {
            visitedNodes.insert(headA);
            headA = headA->next;
        }

        // Check for intersection in list B
        while (headB != nullptr) {
            if (visitedNodes.find(headB) != visitedNodes.end()) {
                return headB; // Intersection found
            }
            headB = headB->next;
        }

        return nullptr; // No intersection
    }
};
```

## Approach 3: Two Pointers (Optimal)

### Solution
```cpp
// Time Complexity: O(m + n)
// Space Complexity: O(1)
class Solution {
public:
    ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {
        if (headA == nullptr || headB == nullptr) {
            return nullptr;
        }

        ListNode* pointerA = headA;
        ListNode* pointerB = headB;

        // Traverse both lists
        while (pointerA != pointerB) {
            // Switch to the other list when reaching the end
            pointerA = (pointerA == nullptr) ? headB : pointerA->next;
            pointerB = (pointerB == nullptr) ? headA : pointerB->next;
        }

        return pointerA; // Either intersection node or nullptr
    }
};
```

