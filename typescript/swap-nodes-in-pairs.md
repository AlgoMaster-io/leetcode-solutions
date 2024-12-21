# [Leetcode 24: Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

## Approaches
- [Approach 1: Iterative Approach](#approach-1-iterative-approach)
- [Approach 2: Recursive Approach](#approach-2-recursive-approach)

### Approach 1: Iterative Approach

#### Intuition:
The iterative approach involves traversing the linked list and swapping adjacent nodes in pairs. By keeping track of the previous node, current node, and the next node, we can perform the necessary pointer updates to achieve the desired swaps. This method does not require any auxiliary space other than a few pointers.

#### Solution:
```typescript
class ListNode {
  val: number;
  next: ListNode | null;

  constructor(val?: number, next?: ListNode | null) {
    this.val = val === undefined ? 0 : val;
    this.next = next === undefined ? null : next;
  }
}

function swapPairs(head: ListNode | null): ListNode | null {
  // Dummy node initialization to handle the edge case where we need to swap the head node
  const dummy: ListNode = new ListNode(0, head);
  let prev: ListNode = dummy;

  // Traverse the list while there is a pair to swap
  while (head && head.next) {
    let first: ListNode = head;
    let second: ListNode = head.next;

    // Swapping nodes
    prev.next = second;       // Link previous node to the second node
    first.next = second.next; // Link first node to the node after second
    second.next = first;      // Link second node to the first node

    // Move the pointers two nodes ahead
    prev = first;
    head = first.next;
  }

  // Return the new head which is dummy.next
  return dummy.next;
}
```

#### Time Complexity:
- **O(n)**, where n is the number of nodes in the linked list. This is because each node is processed exactly once.

#### Space Complexity:
- **O(1)**, as we are only using a fixed amount of extra space.

### Approach 2: Recursive Approach

#### Intuition:
In the recursive approach, we handle the first pair of nodes and then rely on recursion to swap the rest. Each pair swap is done by changing the `next` pointers of the nodes. The recursion will terminate when there are no more pairs to swap, starting the unwinding process where the pointers are switched correctly.

#### Solution:
```typescript
function swapPairsRecursive(head: ListNode | null): ListNode | null {
  // Base case: If we have no node or only one node left, return the node itself
  if (!head || !head.next) { 
    return head; 
  }

  // Nodes to be swapped
  const firstNode: ListNode = head;
  const secondNode: ListNode = head.next;

  // Recursively call function for the rest of the list beyond the second node
  firstNode.next = swapPairsRecursive(secondNode.next);

  // Actual swapping
  secondNode.next = firstNode;

  // Now secondNode is the head of the swapped pair
  return secondNode;
}
```

#### Time Complexity:
- **O(n)**, where n is the number of nodes in the linked list. Each node is visited once in the recursive call stack.

#### Space Complexity:
- **O(n)**, because of the recursion stack. The recursion stack can go as deep as the number of nodes in the worst case.

