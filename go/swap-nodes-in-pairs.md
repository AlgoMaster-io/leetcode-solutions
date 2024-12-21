# [Leetcode 24: Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

## Approaches
- [Approach 1: Iterative Approach](#approach-1-iterative-approach)
- [Approach 2: Recursive Approach](#approach-2-recursive-approach)

### Approach 1: Iterative Approach

#### Intuition:
The iterative approach involves traversing the linked list and swapping nodes in pairs. We can achieve this by maintaining pointers to the previous node, the current node, and the next node pair that needs to be swapped.

#### Detailed Steps:
1. Create a dummy node to simplify edge cases such as swapping the first two nodes.
2. Use a pointer `prev` which starts at the dummy node.
3. Traverse the list using a loop, and in each iteration:
   - Identify two nodes to be swapped, `first` and `second`.
   - Update the pointers to swap these two nodes.
   - Move the `prev` pointer two nodes ahead to proceed with the next pair.
4. Continue until there are no more pairs to swap.

#### Code Implementation:
```go
// Definition for singly-linked list.
type ListNode struct {
    Val  int
    Next *ListNode
}

func swapPairs(head *ListNode) *ListNode {
    // Dummy node to handle edge cases
    dummy := &ListNode{Next: head}
    prev := dummy

    for prev.Next != nil && prev.Next.Next != nil {
        // Nodes to be swapped
        first := prev.Next
        second := prev.Next.Next

        // Swapping nodes
        first.Next = second.Next
        second.Next = first
        prev.Next = second

        // Move prev pointer two nodes ahead
        prev = first
    }

    return dummy.Next
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of nodes in the linked list. We are iterating through the list once.
- **Space Complexity**: O(1), as we are using a constant amount of space.

---

### Approach 2: Recursive Approach

#### Intuition:
The recursive approach makes use of the call stack to handle the node swapping. The base idea is to swap the first two nodes and then recursively call the function for the remaining list.

#### Detailed Steps:
1. Base case: If the list is empty or has only one node, return the list as is.
2. Identify the first two nodes of the list: `first` and `second`.
3. Perform the swap by pointing the `first` node's `next` to the result of the recursive call on the list starting from `third`.
4. Point the `second` node’s `next` to `first`.
5. Return `second`, as it is the new head of the swapped pair.

#### Code Implementation:
```go
func swapPairsRecursive(head *ListNode) *ListNode {
    // Base case
    if head == nil || head.Next == nil {
        return head
    }

    // Nodes to be swapped
    first := head
    second := head.Next

    // Perform the swap and recursive call
    first.Next = swapPairsRecursive(second.Next)
    second.Next = first

    // Return the new head after swap
    return second
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of nodes in the linked list. Each node is visited exactly once.
- **Space Complexity**: O(n), due to the recursive call stack. 

These approaches solve the problem of swapping nodes in pairs both iteratively and recursively, providing a clear understanding of each method's workings.

