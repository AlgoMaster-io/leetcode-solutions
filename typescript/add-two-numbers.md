# [Leetcode 2: Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Approaches
- [Approach 1: Iterative Linked List Traversal](#approach-1-iterative-linked-list-traversal)

---

## Approach 1: Iterative Linked List Traversal

### Intuition
The problem involves adding two numbers represented by linked lists. Each node contains a single digit and the digits are stored in reverse order. To solve the problem, we need to simulate the addition process, iterating through the list nodes, adding the digits, and managing any carry-over from one digit addition to the next.

### Steps:
1. Initialize a dummy node `dummyHead` to help easily return the result list and a pointer `curr` which points to the current node in the result list.
2. Use a variable `carry` to keep track of the carry-over (initially set to 0).
3. Use two pointers, `p` and `q`, to traverse `l1` and `l2` respectively.
4. Iterate over the lists while there are nodes to process or there's a carry value:
   - Extract the current values from `l1` and `l2` (or use 0 if the list is exhausted).
   - Compute the sum of these values plus the carry.
   - Update the carry for the next step.
   - Create a new node with the digit part of the sum and attach it to the `curr.next`.
   - Move the `curr` pointer to the next node.
   - Move the `p` and `q` pointers to their respective next nodes if they exist.
5. After the end of the loop, if a carry remains, create a new node with this carry value.
6. Return `dummyHead.next` since `dummyHead` was an auxiliary node.

### Time Complexity
- **Time Complexity:** O(max(m, n)), where m and n are the lengths of the two lists. We iterate through both lists simultaneously.
- **Space Complexity:** O(max(m, n)), the length of the result list will be at most max(m, n) + 1.

### Code

```typescript
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    let dummyHead = new ListNode(0); // Create a dummy node to help in easily returning the head of the result list
    let p = l1, q = l2, curr = dummyHead; // Initialize pointers
    let carry = 0; // Initialize carry to 0

    // Loop through both lists until both lists have been fully processed
    while (p !== null || q !== null) {
        let x = (p !== null) ? p.val : 0; // Get the current value of l1, or 0 if p has reached the end
        let y = (q !== null) ? q.val : 0; // Get the current value of l2, or 0 if q has reached the end
        let sum = carry + x + y; // Calculate the sum of the two digits and the carry
        carry = Math.floor(sum / 10); // Update the carry for the next iteration
        curr.next = new ListNode(sum % 10); // Create a new node with the remainder of the sum
        curr = curr.next; // Move to the next node in the result list

        if (p !== null) p = p.next; // Advance p if it hasn't reached the end
        if (q !== null) q = q.next; // Advance q if it hasn't reached the end
    }
    
    // Check if there's any carry left after processing both lists, add it as a last node if needed
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }

    // Return the next of dummy node, which is the head of the resultant linked list
    return dummyHead.next;
}
```


