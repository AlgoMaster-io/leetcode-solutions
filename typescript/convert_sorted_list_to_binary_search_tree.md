# [109. Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approach 1: Recursion with Slow and Fast Pointers

### Solution
typescript
```typescript
// Time Complexity: O(n log n), where n is the length of the list
// Space Complexity: O(log n) (due to recursion stack)

class ListNode {
    val: number;
    next: ListNode | null = null;
    constructor(val: number) {
        this.val = val;
    }
}

class TreeNode {
    val: number;
    left: TreeNode | null = null;
    right: TreeNode | null = null;
    constructor(val: number) {
        this.val = val;
    }
}

function sortedListToBST(head: ListNode | null): TreeNode | null {
    if (head === null) return null; // Base case: empty list
    if (head.next === null) return new TreeNode(head.val); // Single node

    // Find the middle of the list using slow and fast pointers
    let prev: ListNode | null = null;
    let slow: ListNode | null = head;
    let fast: ListNode | null = head;

    while (fast !== null && fast.next !== null) {
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }

    // Disconnect the left half of the list
    if (prev !== null) prev.next = null;

    // Create the root node
    const root = new TreeNode(slow!.val);

    // Recursively build the left and right subtrees
    root.left = sortedListToBST(head);
    root.right = sortedListToBST(slow!.next);

    return root;
}
```

## Approach 2: In-Order Simulation with List Conversion

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)

class ListNode {
    val: number;
    next: ListNode | null = null;
    constructor(val: number) {
        this.val = val;
    }
}

class TreeNode {
    val: number;
    left: TreeNode | null = null;
    right: TreeNode | null = null;
    constructor(val: number) {
        this.val = val;
    }
}

function sortedListToBST(head: ListNode | null): TreeNode | null {
    const values: number[] = [];
    
    // Convert the linked list to an array for easier access
    while (head !== null) {
        values.push(head.val);
        head = head.next;
    }

    // Recursively construct the BST
    return buildBST(values, 0, values.length - 1);
}

function buildBST(values: number[], left: number, right: number): TreeNode | null {
    if (left > right) return null; // Base case

    const mid = left + Math.floor((right - left) / 2); // Find the middle element
    const root = new TreeNode(values[mid]); // Create the root node

    // Recursively build left and right subtrees
    root.left = buildBST(values, left, mid - 1);
    root.right = buildBST(values, mid + 1, right);

    return root;
}
```

