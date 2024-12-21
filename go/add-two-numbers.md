# [LeetCode Problem 2: Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Approaches

1. [Elementary Addition with Carry](#elementary-addition-with-carry)
2. [Optimized Version](#optimized-version)

---

### Elementary Addition with Carry

**Intuition:**

This problem involves simulating the addition of two numbers represented as linked lists, where each node contains a single digit. To solve this:

- Traverse both linked lists simultaneously, starting from the head (least significant digit).
- For each pair of nodes (digits), compute the sum.
- Handle the carry to the next higher digit if the sum exceeds 9.
- Continue this till both linked lists and the carry are exhausted.

**Steps:**

1. Initialize a dummy head node (to simplify edge cases) and a pointer to traverse the new list.
2. Traverse both linked lists simultaneously. For each node, calculate:
   - The sum of the two digits and any carry from the previous step.
   - The new carry for the next iteration.
   - The new digit to store in the result list.
3. Append a new node with the calculated digit to the result list.
4. Continue until both lists are exhausted.
5. If a carry remains after the final addition, add a new node with that carry value.

**Time Complexity:** O(max(n, m)), where n and m are the lengths of the two linked lists.

**Space Complexity:** O(max(n, m)), since the maximum length of the result list will be one more than the longer input list if there is a final carry.

```go
package main

import "fmt"

// ListNode defines a singly linked list.
type ListNode struct {
    Val  int
    Next *ListNode
}

func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := &ListNode{} // Initialize a dummy node to facilitate result list construction.
    current := dummy     // Pointer to construct our new linked list.
    carry := 0           // Initialize carry for addition.

    // Traverse both lists.
    for l1 != nil || l2 != nil {
        x, y := 0, 0
        if l1 != nil {
            x = l1.Val
            l1 = l1.Next
        }
        if l2 != nil {
            y = l2.Val
            l2 = l2.Next
        }

        sum := x + y + carry // Compute sum of values and carry.
        carry = sum / 10     // Compute new carry.
        current.Next = &ListNode{Val: sum % 10} // Create new node with the current digit.
        current = current.Next // Move current pointer.
    }

    // If carry exists after the final addition, append it as a new node.
    if carry > 0 {
        current.Next = &ListNode{Val: carry}
    }

    return dummy.Next // Skip the dummy node to return the result.
}

// Helper function to print the list - for testing purposes.
func printList(node *ListNode) {
    for node != nil {
        fmt.Print(node.Val)
        node = node.Next
        if node != nil {
            fmt.Print(" -> ")
        }
    }
    fmt.Println()
}

// Example usage
func main() {
    l1 := &ListNode{2, &ListNode{4, &ListNode{3, nil}}}
    l2 := &ListNode{5, &ListNode{6, &ListNode{4, nil}}}
    result := addTwoNumbers(l1, l2)
    printList(result) // Output: 7 -> 0 -> 8
}
```

### Optimized Version

The optimized version follows the same steps as the basic approach but with more efficient handling of edge cases and potentially reducing memory allocation overhead.

In this particular problem, the initial elementary solution is already optimal in terms of time complexity since each node is processed exactly once. Any further optimization would be minimal with the given problem constraints.

Therefore, it's recommended to focus on edge cases and code clarity:

- Consider what happens when one list is significantly longer than the other.
- Handle cases where the input lists include leading zeros.
- Ensure that the final carry is correctly appended as a new node if needed.

There's no direct faster solution in terms of big-O notation, but always strive to have clean, concise code with comprehensive comments to explain each critical line of the algorithm.

---

In conclusion, this problem illustrates a basic linked list manipulation where the core idea is to correctly manage node traversal and ensure the carry-over during addition, mimicking elementary school arithmetic. The given solutions handle typical edge cases such as differing list lengths and leftover carry efficiently.

