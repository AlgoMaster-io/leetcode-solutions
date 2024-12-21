# [Leetcode 109: Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approaches
- [Approach 1: Convert List to Array and Use Recursion](#approach-1)
- [Approach 2: Inorder Simulation using Recursion](#approach-2)

### Approach 1: Convert List to Array and Use Recursion

**Intuition:**

The sorted linked list is given, and we need to convert it into a balanced binary search tree (BST), where each node follows the properties of a BST. A BST has a property where each node has the left subtree with values less than the node and right with values greater. 

An easy approach is to convert the linked list into an array for easy random access, and then apply the classic approach of converting a sorted array to a BST using recursion. With a sorted array, we can easily find the middle element, which will be the root of the BST. The elements to the left of the middle will be in the left subtree, and the elements to the right will be in the right subtree.

**Steps:**

1. Convert the linked list to an array for easy access to the middle element.
2. Use a helper function to recursively construct the BST:
   - Base case: if the start index is greater than the end index, return `nullptr`.
   - Calculate the middle index, and create a TreeNode with the middle element.
   - Recursively construct the left subtree from the left half of the array.
   - Recursively construct the right subtree from the right half of the array.
3. Return the root of the BST.

```cpp
// Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

// Definition for binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if (!head) return nullptr;
        
        // Step 1: Convert the linked list to an array
        std::vector<int> values;
        ListNode* current = head;
        while (current) {
            values.push_back(current->val);
            current = current->next;
        }
        
        // Step 2: Recursive function to build the BST
        function<TreeNode*(int, int)> buildTree = [&](int start, int end) -> TreeNode* {
            // Base case
            if (start > end) return nullptr;
            
            // Find the middle element
            int mid = start + (end - start) / 2;
            TreeNode* node = new TreeNode(values[mid]);
            
            // Recursively construct the left and right subtrees
            node->left = buildTree(start, mid - 1);
            node->right = buildTree(mid + 1, end);
            
            return node;
        };
        
        return buildTree(0, values.size() - 1);
    }
};

// Time Complexity: O(N), where N is the number of nodes in the linked list (conversion to array is O(N) and constructing BST is O(N)).
// Space Complexity: O(N) for storing the linked list elements in an array.
```

### Approach 2: Inorder Simulation using Recursion

**Intuition:**

Instead of converting the list to an array, we simulate inorder traversal using the fast and slow pointer technique to find the middle node directly in the list. This avoids the extra space used in the array conversion from Approach 1. This technique leverages the properties of inorder traversal in a BST where the inorder sequence is sorted - matching the sequence from the sorted linked list.

**Steps:**

1. Use the fast-slow pointer technique to find the middle element of the linked list, which represents the root of the BST.
2. Make recursive calls for constructing the left and right subtree.
3. Special cases:
   - if the list has only one element, return it as a leaf TreeNode.
   - Use two pointers: `prevPtr` to keep track of node before middle, and `slowPtr` to find the middle and split the list.
4. Recursively handle the sub-lists on either side of the mid node to form balanced left and right subtrees.

```cpp
// Reuse ListNode and TreeNode structures from Approach 1
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if (!head) return nullptr;
        
        // Function to find the middle element of the linked list
        auto findMiddle = [](ListNode* start) {
            ListNode *prevPtr = nullptr, *slowPtr = start, *fastPtr = start;
            
            // Move fastPtr two steps and slowPtr one step in each iteration
            while (fastPtr && fastPtr->next) {
                prevPtr = slowPtr;
                slowPtr = slowPtr->next;
                fastPtr = fastPtr->next->next;
            }
            // Disconnect left half
            if (prevPtr) prevPtr->next = nullptr;
            
            return slowPtr;
        };
        
        // The base case when there's a single node in the list
        ListNode* mid = findMiddle(head);
        
        TreeNode* root = new TreeNode(mid->val);
        
        // Base case for recursion
        if (head == mid) {
            return root;
        }
        
        // Recursively form balanced subtrees
        root->left = sortedListToBST(head);
        root->right = sortedListToBST(mid->next);
        
        return root;
    }
};

// Time Complexity: O(N log N), each level of recursing requires a traversal of length N/2, producing O(log N) depth.
// Space Complexity: O(log N) for recursion stack in balanced tree.
```

These approaches meticulously demonstrate both intuitive and optimized means of solving the Sorted List to BST problem. The second approach is efficient in both time and space as it simulates inorder traversal directly on the linked list.

