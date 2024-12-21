# [Leetcode 21: Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

## Solutions
- [Approach 1: Recursive Solution](#approach-1)
- [Approach 2: Iterative Solution](#approach-2)
- [Approach 3: In-place Iterative Solution](#approach-3)

## Approach 1: Recursive Solution

### Intuition
The problem involves merging two sorted linked lists into a single sorted linked list. A recursive approach can work elegantly here by comparing the heads of the two lists. The smaller node is chosen as the current node of the resultant list, and the recursion is applied to the next of this chosen node along with the other list. 

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

function mergeTwoLists(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    // Base cases: if either list is empty, return the other list
    if (!l1) return l2;
    if (!l2) return l1;

    // Compare the current nodes of both lists
    if (l1.val < l2.val) {
        // l1 is smaller, so it should be the current node of merged list
        l1.next = mergeTwoLists(l1.next, l2);  // Recurse with the next of l1
        return l1;
    } else {
        // l2 is smaller or equal, so it should be the current node of merged list
        l2.next = mergeTwoLists(l1, l2.next);  // Recurse with the next of l2
        return l2;
    }
}
```

### Complexity
- **Time Complexity:** O(n + m) where n and m are the lengths of the two lists.
- **Space Complexity:** O(n + m) due to the recursive call stack space.

## Approach 2: Iterative Solution

### Intuition
To avoid the extra space overhead of recursion, an iterative approach can be used. We maintain a dummy node as a starting point and compare elements of both lists iteratively. We adjust pointers so that the merged list is built by always pointing to the smaller node between the two lists.

### Code
```typescript
function mergeTwoLists(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    const dummy = new ListNode(-1);  // Start with a dummy node to simplify edge cases
    let current = dummy;

    // Iterate as long as there are elements in both lists
    while (l1 !== null && l2 !== null) {
        if (l1.val < l2.val) {
            current.next = l1;  // Point to the smaller node
            l1 = l1.next;  // Move to the next node in l1
        } else {
            current.next = l2;
            l2 = l2.next;  // Move to the next node in l2
        }
        current = current.next;  // Move the pointer of the merged list
    }

    // If there are remaining elements in either list, append them
    current.next = l1 !== null ? l1 : l2;

    return dummy.next;  // Return the next of dummy node, as it is the actual head
}
```

### Complexity
- **Time Complexity:** O(n + m) where n and m are the lengths of the two lists.
- **Space Complexity:** O(1) because we use only constant extra space.

## Approach 3: In-place Iterative Solution

### Intuition
An in-place approach can modify the pointers of the original list to minimize memory usage further. This approach involves less restructuring of data and additional data structures, providing a direct modification method.

### Code
```typescript
function mergeTwoLists(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    if (!l1) return l2;
    if (!l2) return l1;

    // Initialize new head based on first comparison
    let head: ListNode | null = null;
    if (l1.val < l2.val) {
        head = l1;
        l1 = l1.next;
    } else {
        head = l2;
        l2 = l2.next;
    }

    let current = head;

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

    current.next = l1 ? l1 : l2;

    return head;
}
```

### Complexity
- **Time Complexity:** O(n + m) where n and m are the lengths of the two lists.
- **Space Complexity:** O(1) as we only rearrange pointers without using extra space.

