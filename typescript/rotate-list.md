# [Leetcode 61: Rotate List](https://leetcode.com/problems/rotate-list/)

## Approaches
1. [Basic Approach: Multiple Rotations by Shifting](#approach-1)
2. [Optimized Approach: Compute Effective Rotations Using Length](#approach-2)
3. [Most Optimal Approach: Connect to Circular List](#approach-3)

---

### Approach 1: Basic Approach: Multiple Rotations by Shifting

#### Intuition
The most straightforward way to rotate a linked list is by shifting the elements one by one for `k` times. This involves removing the last element of the list and inserting it at the front, repeatedly for `k` times. Although it's simple to understand, this approach can be quite inefficient.

#### Steps
1. Traverse the list once to calculate its length.
2. For each rotation, find the tail node and move it to the head.
3. Repeat the above step `k` times.

#### Code
```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = (val===undefined ? 0 : val)
        this.next = (next===undefined ? null : next)
    }
}

function rotateRight(head: ListNode | null, k: number): ListNode | null {
    if (!head || !head.next || k === 0) return head;

    // Calculate the length of the list
    let length = 1;
    let lastNode = head;
    while (lastNode.next) {
        lastNode = lastNode.next;
        length++;
    }

    // Perform `k % length` operations to make it efficient
    k = k % length;
    if (k === 0) return head;

    for (let i = 0; i < k; i++) {
        let current = head;
        let prev = null;

        // Find the second-to-last node
        while (current && current.next) {
            prev = current;
            current = current.next;
        }

        // Rotate the list
        if (prev && current) {
            prev.next = null;
            current.next = head;
            head = current;
        }
    }

    return head;
}
```
#### Complexity
- **Time Complexity:** O(n * k), where `n` is the length of the list and `k` is the number of rotations. Each rotation involves traversing nearly the entire list.
- **Space Complexity:** O(1), as no additional space is used.

---

### Approach 2: Optimized Approach: Compute Effective Rotations Using Length

#### Intuition
Instead of performing `k` rotations, recognize that rotating `k` times is equivalent to rotating `k % length` times. This approach reduces unnecessary rotations.

#### Steps
1. Calculate the length of the list.
2. Adjust `k` to be `k % length`.
3. Find the new head which will be at position `(length - k)`.
4. Reconnect the nodes accordingly.

#### Code
```typescript
function rotateRight(head: ListNode | null, k: number): ListNode | null {
    if (!head || !head.next || k === 0) return head;

    // Calculate the length of the list
    let length = 1;
    let lastNode = head;
    while (lastNode.next) {
        lastNode = lastNode.next;
        length++;
    }

    // Adjust `k` to be within bounds
    k = k % length;
    if (k === 0) return head;

    // Find the new head and tail after the rotation
    let stepsToNewHead = length - k;
    let newTail = head;

    for (let i = 1; i < stepsToNewHead; i++) {
        newTail = newTail.next!;
    }

    let newHead = newTail.next;
    newTail.next = null;
    lastNode.next = head;

    return newHead;
}
```

#### Complexity
- **Time Complexity:** O(n), where `n` is the length of the list. The list is traversed twice.
- **Space Complexity:** O(1), as we only alter pointers.

---

### Approach 3: Most Optimal Approach: Connect to Circular List

#### Intuition
Construct a circular linked list and then cut it at the appropriate location. This optimally moves nodes into their final positions after conversion to a closed loop form.

#### Steps
1. Calculate the length of the list as before.
2. Connect the last node to the head to form a circular list.
3. Find the new head by traversing `(length - k)` nodes.
4. Set the node prior to new-head's next to null to break the loop.

#### Code
```typescript
function rotateRight(head: ListNode | null, k: number): ListNode | null {
    if (!head || !head.next || k === 0) return head;

    // Calculate the length of the list
    let length = 1;
    let lastNode = head;
    while (lastNode.next) {
        lastNode = lastNode.next;
        length++;
    }

    // Link the last node to the first node
    lastNode.next = head;

    // Adjust `k` to be within bounds
    k = k % length;

    // Find the new tail, which is `(length - k - 1)` steps from the beginning
    let stepsToNewTail = length - k;
    let newTail = head;

    for (let i = 1; i < stepsToNewTail; i++) {
        newTail = newTail.next!;
    }

    let newHead = newTail.next;
    newTail.next = null; // Break the loop

    return newHead;
}
```

#### Complexity
- **Time Complexity:** O(n), where `n` is the length of the list. The list is traversed once to calculate its length and then again to adjust pointers.
- **Space Complexity:** O(1), as only a constant amount of space is used for variables.

