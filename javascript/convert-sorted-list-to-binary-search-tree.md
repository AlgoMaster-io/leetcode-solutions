
[Leetcode Problem 109: Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approaches:
1. [Brute Force: Convert List to Array](#brute-force-convert-list-to-array)
2. [Optimal In-place Conversion](#optimal-in-place-conversion)

### Brute Force: Convert List to Array

#### Intuition:
This brute force method involves converting the linked list into an array, and then leveraging the properties of Binary Search Tree (BST) to build the BST recursively. The central concept is that for balanced BST, the middle element of a sorted list/array becomes the root. This method simplifies traversing since we can access elements via indices.

#### Steps:
1. Traverse the linked list to convert it into an array.
2. Use a helper method to build a BST by choosing the middle element of the current sub-array as the root.
3. Recursively call the helper method to build the left and right subtrees.

#### Code:
```javascript
// Definition for singly-linked list and tree node is given
function ListNode(val, next) {
    this.val = (val===undefined ? 0 : val)
    this.next = (next===undefined ? null : next)
}

function TreeNode(val, left, right) {
    this.val = (val===undefined ? 0 : val)
    this.left = (left===undefined ? null : left)
    this.right = (right===undefined ? null : right)
}

var sortedListToBST = function(head) {
    let nums = [];
    
    // Convert LinkedList to array
    while (head) {
        nums.push(head.val);
        head = head.next;
    }

    // Helper function to convert array to BST
    function buildBST(left, right) {
        if (left > right) {
            return null; // Base case when no elements are remaining
        }
        
        let mid = Math.floor((left + right) / 2);
        let node = new TreeNode(nums[mid]); // Middle element becomes the root

        node.left = buildBST(left, mid - 1); // Build left subtree
        node.right = buildBST(mid + 1, right); // Build right subtree
        
        return node;
    }
    
    return buildBST(0, nums.length - 1);
};
```

#### Complexity:
- Time Complexity: O(N), where N is the number of nodes in the list. This includes O(N) for converting the list to array and O(N) for constructing the BST.
- Space Complexity: O(N) for storing elements in the array.

### Optimal In-place Conversion

#### Intuition:
The optimal method focuses on reducing space usage by avoiding array conversion. Instead, we'll use a slow and fast pointer technique to find the middle of the list a.k.a the root of our current subtree, and recursively perform the splitting.

#### Steps:
1. Implement a function to find the middle of the current linked list, which acts as the root.
2. Recursively form the left and right subtrees.

#### Code:
```javascript
var sortedListToBST = function(head) {
    if (!head) return null; // Base case for empty list

    function findMiddle(left, right) {
        let slow = left;
        let fast = left;
        
        // Move slow one step and fast two steps
        while (fast !== right && fast.next !== right) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;
    }
    
    function convertListToBST(left, right) {
        if (left === right) {
            return null; // Base case, when left equals right, subtree is empty
        }
        
        let mid = findMiddle(left, right);
        let node = new TreeNode(mid.val); // Middle node becomes the root
        
        node.left = convertListToBST(left, mid); // Left side of mid becomes left subtree
        node.right = convertListToBST(mid.next, right); // Right side of mid becomes right subtree
        
        return node;
    }
    
    return convertListToBST(head, null);
};
```

#### Complexity:
- Time Complexity: O(N log N) because every node in the list contributes to the traversal at each level of recursion.
- Space Complexity: O(log N) due to the recursive stack space used when constructing the BST node by node.

