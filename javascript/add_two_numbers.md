# [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Approach: Iterative Solution with Carry

### Solution
javascript
```javascript
// Time Complexity: O(max(m, n)), where m and n are the lengths of the two lists
// Space Complexity: O(max(m, n)), for the new linked list

// Definition for singly-linked list.
function ListNode(val, next = null) {
    this.val = val;
    this.next = next;
}

const addTwoNumbers = function(l1, l2) {
    let dummy = new ListNode(0); // Dummy node to simplify the process
    let current = dummy; // Pointer to the current node in the result list
    let carry = 0; // Store carry from the addition

    // Traverse both lists
    while (l1 !== null || l2 !== null || carry !== 0) {
        let sum = carry; // Start with the carry

        // Add values from l1 and l2 if they exist
        if (l1 !== null) {
            sum += l1.val;
            l1 = l1.next;
        }
        if (l2 !== null) {
            sum += l2.val;
            l2 = l2.next;
        }

        // Calculate new carry and the digit to store
        carry = Math.floor(sum / 10);
        current.next = new ListNode(sum % 10); // Store the last digit of the sum
        current = current.next; // Move to the next node
    }

    return dummy.next; // Skip the dummy node
};
```

