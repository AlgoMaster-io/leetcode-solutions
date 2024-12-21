# [Leetcode Problem 21: Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

## Approaches

1. [Iterative Approach using Extra List](#approach-1)
2. [Iterative In-Place Merge](#approach-2)
3. [Recursive In-Place Merge](#approach-3)

### Approach 1: Iterative Approach using Extra List

**Intuition:**

The simplest way to merge two sorted linked lists is to create a new dummy linked list and append the smallest node from either list one node at a time until one of the lists is exhausted. After that, append any remaining nodes from the non-exhausted list.

- **Time Complexity:** O(n + m), where n and m are the lengths of the two lists.
- **Space Complexity:** O(n + m), due to the space used to store the new list.

```javascript
function ListNode(val, next = null) {
    this.val = val;
    this.next = next;
}

var mergeTwoLists = function(l1, l2) {
    // Create a dummy node to simplify edge cases.
    let dummy = new ListNode(0);
    let current = dummy;

    // Traverse through both lists and append the smaller value to the result list.
    while (l1 !== null && l2 !== null) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }

    // Attach the remaining part of l1 or l2.
    if (l1 !== null) {
        current.next = l1;
    } else {
        current.next = l2;
    }

    // Return the merged list, starting from the next node of the dummy, since the dummy is a placeholder.
    return dummy.next;
};
```

### Approach 2: Iterative In-Place Merge

**Intuition:**

Instead of creating a new list, we can merge the nodes in place to save space. The logic remains similar: compare the two nodes at a time and link the smaller node to our merged list iterator.

- **Time Complexity:** O(n + m), where n and m are the lengths of the two lists.
- **Space Complexity:** O(1), since we are not using any additional space for merging except for pointers.

```javascript
var mergeTwoLists = function(l1, l2) {
    // Create a dummy node to handle edge cases easily.
    let dummy = new ListNode(0);
    let current = dummy;

    // In-place merge without creating new nodes
    while (l1 !== null && l2 !== null) {
        // Compare and connect nodes.
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }

    // Connect the remaining nodes, if any.
    current.next = l1 !== null ? l1 : l2;

    return dummy.next;
};
```

### Approach 3: Recursive In-Place Merge

**Intuition:**

Leverage the recursion stack to handle merging seamlessly. This approach elegantly merges two lists by first determining the smaller node and recursively calling the function for the next nodes. The base case is when one of the lists is empty, in which case the remainder of the other list is appended.

- **Time Complexity:** O(n + m), where n and m are the lengths of the two lists.
- **Space Complexity:** O(n + m), due to recursion stack usage.

```javascript
var mergeTwoLists = function(l1, l2) {
    // Base case: if one list is empty, return the other list.
    if (l1 === null) return l2;
    if (l2 === null) return l1;

    // Recursively determine which node is smaller, and set the next reference accordingly.
    if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
};
```

Each approach has its own strengths, from simplicity and clarity to optimal space usage, making "Merge Two Sorted Lists" a great problem to understand the trade-offs in linked list manipulation.

