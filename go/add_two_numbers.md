# [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Approach: Iterative Solution with Carry

### Solution
```go
// Time Complexity: O(max(m, n)), where m and n are the lengths of the two lists
// Space Complexity: O(max(m, n)), for the new linked list
package main

// Definition for singly-linked list.
type ListNode struct {
    Val  int
    Next *ListNode
}

func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := &ListNode{0, nil} // Dummy node to simplify the process
    current := dummy           // Pointer to the current node in the result list
    carry := 0                 // Store carry from the addition

    // Traverse both lists
    for l1 != nil || l2 != nil || carry != 0 {
        sum := carry // Start with the carry

        // Add values from l1 and l2 if they exist
        if l1 != nil {
            sum += l1.Val
            l1 = l1.Next
        }
        if l2 != nil {
            sum += l2.Val
            l2 = l2.Next
        }

        // Calculate new carry and the digit to store
        carry = sum / 10
        current.Next = &ListNode{sum % 10, nil} // Store the last digit of the sum
        current = current.Next // Move to the next node
    }

    return dummy.Next // Skip the dummy node
}
```

