# [Leetcode 2: Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Approaches
- [Approach 1: Elementary Math Approach](#approach-1-elementary-math-approach)

## Approach 1: Elementary Math Approach

### Intuition
To solve the problem of adding two numbers represented as linked lists, we need to simulate the process of addition just like when adding numbers manually. We'll traverse both linked lists from the least significant digit to the most significant one, adding each pair of digits along with any carry from the previous addition. This is the straightforward and direct approach to solving the problem.

### Steps:
1. Initialize a dummy node to help us easily return the result list later.
2. Use a `current` pointer starting at the dummy node to build the result list.
3. Use a `carry` variable initialized to zero to track any overflow from each sum operation.
4. Traverse both linked lists until you have processed all nodes from both lists.
5. For each node pair, add their values and the carry from the previous step.
6. Use division and modulus to update the carry and the output digit.
7. If a carry exists after finishing the traversal, append it as a new node.
8. Return the linked list starting from the node next to the dummy node.

### C# Code
```csharp
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
public class Solution {
    public ListNode AddTwoNumbers(ListNode l1, ListNode l2) {
        // Dummy node to build the resulting linked list
        ListNode dummyHead = new ListNode(0);
        // Pointer to construct the new list
        ListNode current = dummyHead;
        
        // Initialize carry to 0 for the first addition
        int carry = 0;

        // Traverse both lists
        while (l1 != null || l2 != null) {
            // Get values from the current nodes
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;

            // Calculate sum of the current digits plus the carry
            int sum = carry + x + y;

            // Update carry for the next step
            carry = sum / 10;

            // Create new node with the digit part of the sum
            current.next = new ListNode(sum % 10);
            current = current.next;

            // Move to the next nodes in the input lists
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }

        // If carry is non-zero, add a new node with the carry value
        if (carry > 0) {
            current.next = new ListNode(carry);
        }

        // Return the next node of dummy head which is the head of the output list
        return dummyHead.next;
    }
}
```

### Complexity Analysis
- **Time Complexity:** O(max(m, n)), where m is the length of l1 and n is the length of l2. Each node in both lists is processed exactly once.
- **Space Complexity:** O(max(m, n)) for the new list storing the sum, as we are creating a new node for each of the digits in the resulting number.

