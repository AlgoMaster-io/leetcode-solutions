# [109. Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approach 1: Recursion with Slow and Fast Pointers

### Solution
```cpp
// Time Complexity: O(n log n), where n is the length of the list
// Space Complexity: O(log n) (due to recursion stack)

class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if (head == nullptr) return nullptr; // Base case: empty list
        if (head->next == nullptr) return new TreeNode(head->val); // Single node
        
        // Find the middle of the list using slow and fast pointers
        ListNode* prev = nullptr;
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != nullptr && fast->next != nullptr) {
            prev = slow;
            slow = slow->next;
            fast = fast->next->next;
        }

        // Disconnect the left half of the list
        if (prev != nullptr) prev->next = nullptr;

        // Create the root node
        TreeNode* root = new TreeNode(slow->val);

        // Recursively build the left and right subtrees
        root->left = sortedListToBST(head);
        root->right = sortedListToBST(slow->next);

        return root;
    }
};
```

## Approach 2: In-Order Simulation with List Conversion

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)

#include <vector>

class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        // Convert the linked list to a vector for easier access
        std::vector<int> values;
        while (head != nullptr) {
            values.push_back(head->val);
            head = head->next;
        }

        // Recursively construct the BST
        return buildBST(values, 0, values.size() - 1);
    }

private:
    TreeNode* buildBST(const std::vector<int>& values, int left, int right) {
        if (left > right) return nullptr; // Base case

        int mid = left + (right - left) / 2; // Find the middle element
        TreeNode* root = new TreeNode(values[mid]); // Create the root node

        // Recursively build left and right subtrees
        root->left = buildBST(values, left, mid - 1);
        root->right = buildBST(values, mid + 1, right);

        return root;
    }
};
```

