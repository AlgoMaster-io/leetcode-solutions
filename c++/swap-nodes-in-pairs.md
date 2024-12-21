# [LeetCode Problem 24: Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

## Solutions

- [Approach 1: Iterative Approach with a Dummy Node](#approach-1-iterative-approach-with-a-dummy-node)
- [Approach 2: Recursive Approach](#approach-2-recursive-approach)

---

### Approach 1: Iterative Approach with a Dummy Node

**Intuition:**

In this approach, we will use iteration combined with a dummy node to simplify handling edge cases (e.g., when the list is empty or has only one node). The dummy node helps manage the new head of the linked list easily after swapping the nodes.

**Steps:**

1. Initialize a dummy node which points to the head of the list. This helps in managing the new head after swapping nodes.
2. Use a pointer `current` starting at the dummy node.
3. While there are at least two nodes to be swapped (i.e., `current.next` and `current.next.next` exist):
   - Identify the two nodes to be swapped: let's call them `first` (current.next) and `second` (current.next.next).
   - Adjust pointers to swap these nodes.
   - Move the `current` pointer two nodes forward.
4. Return the list starting from `dummy.next`, which is the new head.

**Time Complexity:**

- O(n): Each pair of nodes is processed in constant time, so the time complexity is linear with respect to the number of nodes.

**Space Complexity:**

- O(1): The space complexity is constant since no extra data structures are used.

```cpp
// Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        // Create a dummy node that serves as the start pointer.
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;

        // Initialize current pointer to the dummy node.
        ListNode* current = dummy;
        
        // Traverse the list while there are at least two nodes left to swap.
        while (current->next != nullptr && current->next->next != nullptr) {
            // Identify the two nodes to be swapped.
            ListNode* first = current->next;
            ListNode* second = current->next->next;
            
            // Adjust the pointers to swap nodes.
            first->next = second->next;
            second->next = first;
            current->next = second;
            
            // Move the pointer two nodes forward for the next swaps.
            current = first;
        }
        
        // Return the new head of the swapped list starting from dummy's next node.
        return dummy->next;
    }
};
```

---

### Approach 2: Recursive Approach

**Intuition:**

The recursive approach mirrors the iterative logic but uses the call stack to handle the node swaps. The base condition when the recursion stops is when there are fewer than two nodes remaining in the section of the list to consider.

**Steps:**

1. If the list is empty or has only one node, return the head.
2. Identify the two nodes to swap: the first is the `head`, and the second is `head->next`.
3. Recursively call the function to swap the remaining list after the second node.
4. Adjust pointers, swapping the `first` and `second` nodes.
5. Return the `second` node, as it is now the head of the swapped pair.

**Time Complexity:**

- O(n): Each recursive call processes a pair of nodes, resulting in linear time complexity.

**Space Complexity:**

- O(n): The space complexity is linear due to the recursion stack.

```cpp
// Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        // Base case: if there are 0 or 1 nodes left, no swap is needed.
        if (!head || !head->next) return head;
        
        // Identify the two nodes to swap.
        ListNode* first = head;
        ListNode* second = head->next;
        
        // Recurse for the list starting after the second node.
        first->next = swapPairs(second->next);
        
        // Swap the first and second nodes.
        second->next = first;
        
        // Return the new head of the list, which is the second node.
        return second;
    }
};
```

Each approach has its advantages, with the iterative being more space-efficient due to avoiding recursion, while the recursive approach offers a cleaner and more natural expression of the problem's logic. Choose based on preference or specific constraints.

