# [Leetcode 234: Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

### Approach:
1. [Reverse and Compare](#reverse-and-compare)
2. [Fast and Slow Pointers with Reversal in Place](#fast-and-slow-pointers-with-reversal-in-place)

---

### Reverse and Compare

#### Intuition:
To determine if a linked list is a palindrome, a straightforward approach is to reverse the linked list and then compare it to the original. If both the original and the reversed list are identical, the list is a palindrome.

#### Steps:
1. Reverse the linked list.
2. Compare the original list with the reversed list node by node.
3. If all nodes are equal, the list is a palindrome.

#### Code:
```typescript
class ListNode {
  val: number;
  next: ListNode | null;
  constructor(val?: number, next?: ListNode | null) {
    this.val = val === undefined ? 0 : val;
    this.next = next === undefined ? null : next;
  }
}

function isPalindrome(head: ListNode | null): boolean {
  if (!head) return true;

  const reverseList = (node: ListNode | null): ListNode | null => {
    let prev: ListNode | null = null;
    let curr = node;
    while (curr !== null) {
      let nextTemp = curr.next; // Store next node
      curr.next = prev;         // Reverse current node's pointer
      prev = curr;              // Move prev to current
      curr = nextTemp;          // Move current to next node
    }
    return prev;
  };

  const reversedHead = reverseList(head);

  let p1 = head;
  let p2 = reversedHead;
  while (p1 !== null && p2 !== null) {
    if (p1.val !== p2.val) return false;
    p1 = p1.next;
    p2 = p2.next;
  }

  return true;
}
```

#### Complexity:
- **Time Complexity**: O(n), where n is the number of nodes in the linked list.
- **Space Complexity**: O(n), as a separate reversed list is created.

---

### Fast and Slow Pointers with Reversal in Place

#### Intuition:
This approach uses the slow and fast pointer technique to find the midpoint of the linked list. The list is then split into two, reversing the second half in place. Finally, both halves are compared for palindrome verification.

#### Steps:
1. Use two pointers, slow and fast, to reach the midpoint where the slow moves one step at a time and fast moves two steps.
2. Reverse the second half of the list.
3. Compare the first half and the reversed second half node by node.
4. If all nodes are equal, the list is a palindrome.

#### Code:
```typescript
function isPalindrome(head: ListNode | null): boolean {
  if (!head || !head.next) return true;

  let slow = head;
  let fast = head;
  let prev: ListNode | null = null;

  // Find the midpoint
  while (fast && fast.next) {
    fast = fast.next.next;
    [slow, prev, slow.next] = [slow.next, slow, prev];
  }

  let secondHalf = fast ? slow.next : slow; // For odd length move slow one step further

  // Compare the first half with the reversed second half
  while (secondHalf) {
    if (prev!.val !== secondHalf.val) return false;
    prev = prev!.next;
    secondHalf = secondHalf.next;
  }

  return true;
}
```

#### Complexity:
- **Time Complexity**: O(n), where n is the number of nodes in the linked list.
- **Space Complexity**: O(1), as the operation is done in place with no extra data structures.

These approaches offer solutions from a basic reverse and compare method to a more optimal in-place comparison, balancing time and space efficiencies effectively.

