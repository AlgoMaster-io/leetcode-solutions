# [Leetcode Problem 2: Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Approaches
- [Approach 1: Basic Iterative Solution](#approach-1-basic-iterative-solution)
- [Approach 2: Optimized Iterative Solution](#approach-2-optimized-iterative-solution)

## Approach 1: Basic Iterative Solution

### Intuition
The problem is asking us to handle the addition of two numbers represented by linked lists, where each node contains a single digit. We'll need to traverse the lists, node by node, while performing digit-wise addition, and handle any carryovers.

### Solution
- Initialize a dummy head node to simplify result list operations.
- Use pointers to traverse the two input linked lists (`l1` and `l2`) and a carry variable initially set to 0.
- Iterate as long as there are remaining nodes in either linked list or there is a carry to be added.
- For each pair of digits from `l1` and `l2`, sum them along with the carry.
- Calculate the new digit and update the carry.
- Append the new digit to the result list and move to the next digits in the linked lists.
- Handle any remaining carry at the end of the iterations.

### Code
```javascript
function addTwoNumbers(l1, l2) {
    let dummyHead = new ListNode(0);  // Initialize a dummy head for ease of result list operations
    let current = dummyHead; // Pointer to add new nodes
    let carry = 0;  // Initialize carry to handle sum of digits more than 9

    while (l1 !== null || l2 !== null || carry !== 0) {  // Continue until both lists are exhausted and no carry
        let sum = carry;  // Start each sum with the carry

        if (l1 !== null) {
            sum += l1.val;  // Add value from l1 if available
            l1 = l1.next;  // Move to the next node
        }
        if (l2 !== null) {
            sum += l2.val;  // Add value from l2 if available
            l2 = l2.next;  // Move to the next node
        }

        carry = Math.floor(sum / 10);  // Update carry
        current.next = new ListNode(sum % 10);  // Create a new node with the digit part of the sum
        current = current.next;  // Move to the next node in the result list
    }

    return dummyHead.next;  // Return the sum list, skipping the dummy head
}
```

### Complexity
- **Time Complexity**: `O(max(m,n))`, where `m` and `n` are the lengths of the two linked lists. We iterate over each list node.
- **Space Complexity**: `O(max(m,n))`, the space is used for the result list.

## Approach 2: Optimized Iterative Solution

### Intuition
This approach largely mirrors the basic one but focuses on streamlining variable usage and improving code readability. The logic behind traversing the linked lists and handling carry remains identical.

### Code
```javascript
function addTwoNumbers(l1, l2) {
    const dummyHead = new ListNode(0);
    let currentNode = dummyHead;
    let carry = 0;

    while (l1 || l2 || carry) {
        const value1 = l1 ? l1.val : 0; // Use 0 if we've exhausted l1
        const value2 = l2 ? l2.val : 0; // Use 0 if we've exhausted l2
        const total = value1 + value2 + carry;

        carry = Math.floor(total / 10); // Carry over the tens digit if necessary
        currentNode.next = new ListNode(total % 10); // Keep the unit digit
        currentNode = currentNode.next;

        if (l1) l1 = l1.next; // Advance in l1
        if (l2) l2 = l2.next; // Advance in l2
    }

    return dummyHead.next; // Result is in dummyHead.next
}
```

### Complexity
- **Time Complexity**: `O(max(m,n))`, where `m` and `n` are the lengths of the two linked lists.
- **Space Complexity**: `O(max(m,n))`, considering the space used for the result list.

This solution captures the essence of digit-wise addition using linked lists, where handling of overflows (carry) is the key aspect to be managed diligently.

