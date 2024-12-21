# [Leetcode Problem 109: Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approaches
1. [Recursive Divide and Conquer Approach](#approach-1)
2. [Optimized Two-Pointer Approach](#approach-2)

---

## Approach 1: Recursive Divide and Conquer Approach

### Intuition
The problem can be broken down into constructing a tree such that it has minimal height. The properties of a BST and the nature of a sorted list give us an advantage where the middle element can always be the root of the BST. Consequently, the elements on the left form the left subtree, and the elements on the right form the right subtree.

### Steps
- Find the middle element of the linked list using the slow and fast pointers technique.
- Recursively construct the left subtree using the left half of the list.
- Recursively construct the right subtree using the right half of the list.
- The base case for the recursion will be when the list is empty, returning `null`.

### Time and Space Complexity
- **Time Complexity:** O(n log n), since we split the list at each recursion level and each split of n nodes will take O(n) time.
- **Space Complexity:** O(log n), because of the recursion stack used.

### Code
```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.next = (next === undefined ? null : next);
    }
}

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.left = (left === undefined ? null : left);
        this.right = (right === undefined ? null : right);
    }
}

function sortedListToBST(head: ListNode | null): TreeNode | null {
    if (!head) return null;

    // Helper function to find the middle node of the linked list
    function findMiddle(left: ListNode | null, right: ListNode | null): ListNode | null {
        let slow = left;
        let fast = left;
        
        while (fast !== right && fast.next !== right) {
            slow = slow.next as ListNode;
            fast = fast.next.next as ListNode;
        }
        
        return slow;
    }

    // Recursive helper function to construct the BST
    function convertListToBST(left: ListNode | null, right: ListNode | null): TreeNode | null {
        if (left === right) return null;

        const mid = findMiddle(left, right);
        const node = new TreeNode(mid?.val);

        node.left = convertListToBST(left, mid);
        node.right = convertListToBST(mid?.next || null, right);

        return node;
    }

    return convertListToBST(head, null);
}
```

---

## Approach 2: Optimized Two-Pointer Approach

### Intuition
Instead of recursively splitting the linked list, traverse the linked list and get its size first. Then use an in-order traversal simulation to construct the tree which allows you to construct the tree from the bottom upwards in O(n) time, allowing for the transformation without repeatedly cutting the list.

### Steps
- Calculate the size of the linked list.
- Use a helper function to perform an in-order traversal simulation and construct the tree.
- For each node, compute the left subtree, assign the current node value, and then proceed to construct the right subtree.
- Use a closure or class-level variable to maintain the current head node to avoid having to enumerate the list many times.

### Time and Space Complexity
- **Time Complexity:** O(n), since each element is processed exactly once.
- **Space Complexity:** O(log n) due to the recursion stack used for in-order traversal.

### Code
```typescript
function sortedListToBST(head: ListNode | null): TreeNode | null {
    // Helper function to get the length of the linked list
    function getLength(node: ListNode | null): number {
        let count = 0;
        while (node !== null) {
            count++;
            node = node.next;
        }
        return count;
    }

    function convertListToBST(left: number, right: number): TreeNode | null {
        if (left > right) return null;

        const mid = Math.floor((left + right) / 2);
        
        // First, recurse on the left child
        const leftChild = convertListToBST(left, mid - 1);

        // Process the current node
        const node = new TreeNode(currentNode.val);
        node.left = leftChild;

        // Move to the next ListNode
        currentNode = currentNode.next as ListNode;

        // Recurse on the right child
        node.right = convertListToBST(mid + 1, right);

        return node;
    }

    const size = getLength(head);
    let currentNode = head;
    return convertListToBST(0, size - 1);
}
```


