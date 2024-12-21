# [Leetcode 141: Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approaches

1. [Brute Force: Use a map to store seen nodes](#approach-1)
2. [Two Pointers - Floyd's Tortoise and Hare Algorithm](#approach-2)

---

### Approach 1: Brute Force: Use a map to store seen nodes

The first approach we can consider involves using a hash map to track nodes we've visited. By doing so, when we encounter a node that we've seen before, we identify the cycle's entry point.

#### Intuition:
- Traverse the list while storing visited nodes in a map.
- If we visit a node that's already in the map, it's the start of the cycle.

#### Implementation:

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        // Use unordered_map to store seen nodes
        unordered_map<ListNode*, bool> visited;
        
        ListNode* current = head;
        // Traverse the list
        while (current != nullptr) {
            // If current node was seen before, it's the start of the cycle
            if (visited.find(current) != visited.end()) {
                return current;
            }
            // Mark current node as visited
            visited[current] = true;
            // Move to the next node
            current = current->next;
        }
        // If no cycle is found
        return nullptr;
    }
};
```

#### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the linked list. In the worst case, each node is visited once.
- **Space Complexity**: O(N), because we store each node reference in the map.

---

### Approach 2: Two Pointers - Floyd's Tortoise and Hare Algorithm

A more optimal solution involves using two pointers to detect the cycle without requiring extra space. This approach is often referred to as Floyd's Tortoise and Hare Algorithm.

#### Intuition:
1. Use two pointers (slow and fast). Move the slow pointer by 1 step and the fast pointer by 2 steps.
2. If they meet, a cycle is detected.
3. Initialize a third pointer from the start of the list.
4. Move the third pointer and the meeting point pointer one step at a time; the point where they meet is the start of the cycle.

#### Implementation:

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (!head || !head->next) return nullptr;
        
        ListNode* slow = head;
        ListNode* fast = head;
        
        // Phase 1: Determine if there is a cycle
        do {
            // Move slow by 1 step
            slow = slow->next;
            // Move fast by 2 steps
            if (fast->next && fast->next->next) {
                fast = fast->next->next;
            } else {
                // If fast pointer reaches null, there is no cycle
                return nullptr;
            }
        } while (slow != fast);
        
        // Phase 2: Find the entry point of the cycle
        ListNode* start = head;
        while (start != slow) {
            // Move both pointers by 1 step
            start = start->next;
            slow = slow->next;
        }
        
        // start is now at the cycle's entry point
        return start;
    }
};
```

#### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the list. In the worst case, we visit each node twice.
- **Space Complexity**: O(1), because we only use a constant amount of extra space for pointers.

This two-pointer technique optimally finds the start of the cycle with minimal overhead, making it a preferred solution for this problem.

