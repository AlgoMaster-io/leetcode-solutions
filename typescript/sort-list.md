## LeetCode Problem: [148. Sort List](https://leetcode.com/problems/sort-list/)

### Approaches:
1. [Naive Approach: Convert Linked List to Array, Sort, and Convert Back](#naive-approach-convert-linked-list-to-array-sort-and-convert-back)
2. [Merge Sort In-place (Top-Down)](#merge-sort-in-place-top-down)
3. [Optimal Solution: Merge Sort In-place (Bottom-Up)](#optimal-solution-merge-sort-in-place-bottom-up)

---

### Naive Approach: Convert Linked List to Array, Sort, and Convert Back

#### Intuition:
The simplest way to sort a linked list is to convert it into an array, sort the array using any standard sorting algorithm, and then rebuild the linked list from the sorted array. This approach is straightforward but not optimal in terms of space complexity.

#### Steps:
1. Traverse the linked list and store its elements in an array.
2. Use the built-in sorting method to sort the array.
3. Create a new linked list from the sorted array elements.

```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.next = (next === undefined ? null : next);
    }
}

function sortList(head: ListNode | null): ListNode | null {
    if (!head) return null;

    // Convert linked list to array
    const arr: number[] = [];
    let current = head;
    while (current) {
        arr.push(current.val);
        current = current.next;
    }

    // Sort the array
    arr.sort((a, b) => a - b);

    // Convert array back to sorted linked list
    const dummyHead = new ListNode();
    current = dummyHead;
    for (const value of arr) {
        current.next = new ListNode(value);
        current = current.next;
    }

    return dummyHead.next;
}
```

**Time Complexity**: O(n log n), where n is the number of nodes. (Array sorting dominates the time complexity).

**Space Complexity**: O(n). (Due to the extra space for the array).

---

### Merge Sort In-place (Top-Down)

#### Intuition:
A more efficient approach is to sort the linked list using merge sort. Merge sort is well-suited for linked lists as it doesn't require random access of elements and can be performed in-place, using constant additional space.

#### Steps:
1. Divide the list into two halves using the slow and fast pointer technique.
2. Recursively sort each half.
3. Merge the two sorted halves.

```typescript
function mergeSort(head: ListNode | null): ListNode | null {
    if (!head || !head.next) return head;

    // Finding the middle point
    let slow: ListNode | null = head;
    let fast: ListNode | null = head;
    let prev: ListNode | null = null;

    while (fast && fast.next) {
        prev = slow;
        slow = slow!.next;
        fast = fast.next.next;
    }
    
    prev!.next = null; // Split the list into two halves

    const l1 = mergeSort(head);
    const l2 = mergeSort(slow);

    return merge(l1, l2);
}

function merge(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    const dummyHead = new ListNode();
    let current = dummyHead;

    while (l1 && l2) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }

    current.next = l1 || l2;

    return dummyHead.next;
}

function sortList(head: ListNode | null): ListNode | null {
    return mergeSort(head);
}
```

**Time Complexity**: O(n log n), due to the recursive halving and merging.

**Space Complexity**: O(log n), due to the stack space used by recursion.

---

### Optimal Solution: Merge Sort In-place (Bottom-Up)

#### Intuition:
While the top-down approach works well, it uses extra space because of recursion. We can eliminate this by using an iterative bottom-up approach that sorts the list by progressively doubling the size of sublists being merged. 

#### Steps:
1. Start from small sublists (size of 1), and iteratively merge sublists of increasing sizes.
2. Use iterative and in-place technique for merging.

```typescript
function sortList(head: ListNode | null): ListNode | null {
    if (!head || !head.next) return head;

    // Get the length of the list
    let length = 0;
    let node = head;
    while (node) {
        length++;
        node = node.next;
    }

    const dummyHead = new ListNode(0, head);

    for (let step = 1; step < length; step <<= 1) {
        let current: ListNode | null = dummyHead.next;
        let tail: ListNode = dummyHead;
        
        while (current) {
            // Split steps
            const left: ListNode | null = current;
            const right: ListNode | null = split(left, step);
            current = split(right, step);

            // Merge and get the new tail
            tail = merge2(left, right, tail);
        }
    }

    return dummyHead.next;
}

function split(head: ListNode | null, step: number): ListNode | null {
    for (let i = 1; head && i < step; i++) {
        head = head.next;
    }
    if (!head) return null;

    const next = head.next;
    head.next = null;
    return next;
}

function merge2(l1: ListNode | null, l2: ListNode | null, start: ListNode): ListNode {
    let current = start;

    while (l1 && l2) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }

    current.next = l1 || l2;

    while (current.next) {
        current = current.next;
    }

    return current;
}
```

**Time Complexity**: O(n log n), as it sorts each pair of sublists iteratively doubling the size each time.

**Space Complexity**: O(1), since it's done in-place without recursion or additional data structure except for the necessary pointers.

---

