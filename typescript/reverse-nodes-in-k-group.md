# [Leetcode 25: Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approaches:
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Iterative Approach Using A Stack](#approach-2-iterative-approach-using-a-stack)
- [Approach 3: Optimal In-Place Iterative Approach](#approach-3-optimal-in-place-iterative-approach)

---

## Approach 1: Recursive Approach

### Intuition:
The recursive approach for reversing nodes in k-group leverages the natural function stack to reverse nodes and handle remaining nodes seamlessly. 

1. Check if there are at least `k` nodes left in the list; if there are not, return the current node, as no reversal is needed.
2. If there are `k` nodes, reverse these nodes recursively.
3. Attach the first node of the reversed part to the result of the next recursive call, which handles the rest of the list.

### Time Complexity:
- **O(n)**: Each node is only visited once.

### Space Complexity:
- **O(n/k)**: Due to the depth of the recursion.

```typescript
class ListNode {
  value: number;
  next: ListNode | null = null;
  constructor(value: number) {
    this.value = value;
  }
}

function reverseKGroup(head: ListNode | null, k: number): ListNode | null {
  let count = 0;
  let node = head;

  // Check if there are at least k nodes to reverse
  while (node !== null && count < k) {
    node = node.next;
    count++;
  }

  if (count === k) { // If we have k nodes
    node = reverseKGroup(node, k); // Reverse next k-group and attach it
    while (count > 0) { // Reverse current k-group
      let tmp = head.next;
      head.next = node;
      node = head;
      head = tmp;
      count--;
    }
    head = node; // Link reversed segment
  }

  return head; // Return new head of the reversed list
}
```

---

## Approach 2: Iterative Approach Using A Stack

### Intuition:
We can use a stack to reverse nodes in k-group iteratively. By pushing `k` nodes onto a stack and then popping them off, we get the nodes in reversed order, which can then be linked together. This approach is straightforward and makes use of the LIFO property of stacks.

1. Iterate through the list and push nodes onto a stack until we have `k` nodes or reach the end.
2. If we have `k` nodes, pop them from the stack and attach them to the new list.
3. Continue this process until the entire list is processed.

### Time Complexity:
- **O(n)**: Each node is visited once and pushed/popped from stack at most once.

### Space Complexity:
- **O(k)**: Stack is used to hold up to `k` nodes at a time.

```typescript
function reverseKGroup(head: ListNode | null, k: number): ListNode | null {
  if (head === null || k === 1) return head;

  let dummy = new ListNode(0);
  dummy.next = head;
  let current = dummy;

  while (true) {
    let stack: ListNode[] = [];
    let temp = current.next;
    let count = 0;

    // Fill the stack with k nodes or reach end
    while (count < k && temp !== null) {
      stack.push(temp);
      temp = temp.next;
      count++;
    }

    if (count !== k) break; // If less than k nodes, stop

    // Pop nodes from stack to reverse them
    while (stack.length > 0) {
      current.next = stack.pop()!;
      current = current.next;
    }
    current.next = temp; // Link remaining part
  }

  return dummy.next;
}
```

---

## Approach 3: Optimal In-Place Iterative Approach

### Intuition:
The most optimal method for reversing nodes in k-group is to do so in-place without using extra space. By carefully keeping track of node pointers, we can reverse a segment of nodes directly within the linked list.

1. Traverse the list with two pointers to check for `k` nodes.
2. Reverse the segment of `k` nodes using three pointers to reverse the links.
3. Attach the reversed segment back to the main list and continue the process till the end.

### Time Complexity:
- **O(n)**: Only constant work is done per node.

### Space Complexity:
- **O(1)**: No extra space is used other than pointers.

```typescript
function reverseKGroup(head: ListNode | null, k: number): ListNode | null {
  let dummy = new ListNode(0);
  dummy.next = head;
  let prevGroupEnd = dummy;

  while (true) {
    let kthNode = prevGroupEnd;
    // Find the kth node in the current list
    for (let i = 0; i < k; i++) {
      kthNode = kthNode.next;
      if (kthNode === null) return dummy.next; // If less than k nodes are left, done
    }

    let groupStart = prevGroupEnd.next;
    let nextGroupStart = kthNode.next;

    // Reverse the k-group
    let prev = kthNode.next;
    let current = groupStart;
    while (current !== nextGroupStart) {
      let next = current.next;
      current.next = prev;
      prev = current;
      current = next;
    }

    // Connect the reversed group with the rest of the linked list
    prevGroupEnd.next = kthNode;
    groupStart.next = nextGroupStart;

    // Move to the next group
    prevGroupEnd = groupStart;
  }
}
```

The third approach is the most efficient, achieving reversal in `O(n)` time without requiring additional space beyond a few pointers, making it optimal for this problem.

