# [Leetcode 92: Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

## Table of Contents
- [Approach 1: Iterative Reverse with Manual Handling](#approach-1)
- [Approach 2: Improved Iterative Reverse with Reversal Function](#approach-2)

## Approach 1: Iterative Reverse with Manual Handling
In this approach, we perform the reversal of the linked list section using an iterative process.
- **Intuition**: Traverse the linked list till the start of the section to be reversed. Then, using two pointers, reverse the specified section of the list in place.

### Step-by-Step:
1. Create a `dummy` node which points to the head, to simplify edge cases handling.
2. Traverse the list to find the node just before the start of the reversal section.
3. Reverse the nodes within the specified range using an iterative reversing technique where each node is detached and moved to the front of the reversed section.
4. Reattach the reversed section back to the list.
5. Return the `next` of `dummy`, which will be the new head.

### Code:
```typescript
class ListNode {
  val: number;
  next: ListNode | null = null;

  constructor(val: number) {
    this.val = val;
  }
}

function reverseBetween(head: ListNode | null, left: number, right: number): ListNode | null {
  if (!head || left === right) return head;

  const dummy = new ListNode(0);
  dummy.next = head;
  let prev = dummy;

  // Move prev to the node just before where the reverse starts
  for (let i = 1; i < left; i++) {
    prev = prev.next!;
  }

  // Reverse the sublist from left to right
  let current = prev.next;
  let next: ListNode | null = null;
  for (let i = 0; i < right - left; i++) {
    next = current!.next;
    current!.next = next!.next;
    next!.next = prev.next;
    prev.next = next;
  }

  return dummy.next;
}
```

### Time Complexity:
- O(N), where N is the number of nodes in the linked list.
- We traverse the list a few times but all operations are linear.

### Space Complexity:
- O(1).
- No additional space other than a few pointers.

## Approach 2: Improved Iterative Reverse with Reversal Function
In this approach, we employ a modular function for reversing the list section, making the code cleaner and more modular.
- **Intuition**: Similar to the first approach but abstracting the reversal into its own function, which can be reused or adapted for other problems.

### Step-by-Step:
1. Create a `dummy` node to simplify boundary conditions.
2. Traverse to the node before the reversal starts.
3. Call a helper function to perform the reversal.
4. Reattach the reversed list section to the main list.
5. Return the head through the next of `dummy`.

### Code:
```typescript
function reverse(head: ListNode | null, n: number): ListNode | null {
  let prev: ListNode | null = null;
  let current = head;

  while (n > 0 && current) {
    const next = current.next;
    current.next = prev;
    prev = current;
    current = next;
    n--;
  }

  if (head) {
    head.next = current;
  }

  return prev;
}

function reverseBetweenImproved(head: ListNode | null, left: number, right: number): ListNode | null {
  if (!head || left === right) return head;

  const dummy = new ListNode(0);
  dummy.next = head;
  let prev = dummy;

  // Traverse to one node before the start of reversal
  for (let i = 1; i < left; i++) {
    prev = prev.next!;
  }

  // Reverse the sublist
  prev.next = reverse(prev.next, right - left + 1);

  return dummy.next;
}
```

### Time Complexity:
- O(N), where N is the length of the list; we process each node a constant number of times.

### Space Complexity:
- O(1).
- No extra space used beyond pointer adjustments.

In both approaches, we've managed the pointer changes manually, allowing for efficient in-place reversal with minimal space complexity. The improved solution offers a clean separation of concerns allowing the reversal logic to be encapsulated and maintained separately.

