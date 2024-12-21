## Leetcode Problem: [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

### Approaches:
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Iterative Approach with Dummy Node](#approach-2-iterative-approach-with-dummy-node)

---

### Approach 1: Recursive Approach

#### Intuition:
The problem is to reverse every k nodes in a linked list. A recursive approach is conceptualized by breaking the list into two parts: the first k nodes and the rest of the list. If the first k nodes are available, reverse them, and recursively apply the same procedure for subsequent nodes. Here, the base condition is a situation where there are fewer than k nodes.

#### Steps:
1. Traverse k nodes to check if there are enough nodes to reverse.
2. Reverse the first k nodes.
3. Recursively call the function for the remaining list.
4. Connect the reversed part to the returned head from the recursive call.

```go
// Definition for singly-linked list.
type ListNode struct {
    Val  int
    Next *ListNode
}

func reverseKGroup(head *ListNode, k int) *ListNode {
    // Function to check if there are at least k nodes remaining
    countNodes := func(node *ListNode, k int) bool {
        count := 0
        for node != nil && count < k {
            node = node.Next
            count++
        }
        return count == k
    }

    // Base case: if fewer than k nodes, return head as is
    if !countNodes(head, k) {
        return head
    }

    // Reverse the first k nodes
    var prev, next *ListNode
    cur := head
    count := 0
    for cur != nil && count < k {
        next = cur.Next
        cur.Next = prev
        prev = cur
        cur = next
        count++
    }

    // head is now the last node of the reversed k nodes, connect it to the result of next nodes
    if cur != nil {
        head.Next = reverseKGroup(cur, k)
    }

    // prev is the new head of the reversed list
    return prev
}
```

**Time Complexity:** O(N), where N is the number of nodes in the linked list.

**Space Complexity:** O(N/k) due to the recursion depth.

---

### Approach 2: Iterative Approach with Dummy Node

#### Intuition:
The iterative approach is more complex to implement, but generally more efficient with O(1) space complexity. Use a dummy node to manage the edges, iteratively reverse every k nodes, and use pointers to connect each reversed segment with the next part of the list.

#### Steps:
1. Use a dummy node that points to the head.
2. Iterate through the list, reversing every k nodes.
3. Use a pointer to manage the current part of the list.
4. Reverse the k nodes, and connect to the last reversed segment and the next part of the list.

```go
func reverseKGroup(head *ListNode, k int) *ListNode {
    dummy := &ListNode{Next: head}
    // These pointers help in reversing k nodes and joining with the next part
    prevGroupEnd := dummy

    // Helper function to reverse the sub-list
    reverseSubList := func(start *ListNode, end *ListNode) (*ListNode, *ListNode) {
        var prev, next *ListNode
        cur := start
        for cur != end {
            next = cur.Next
            cur.Next = prev
            prev = cur
            cur = next
        }
        return prev, start
    }

    for {
        groupStart := prevGroupEnd.Next
        // Check enough nodes exist to reverse
        kthNode := prevGroupEnd
        for i := 0; i < k && kthNode != nil; i++ {
            kthNode = kthNode.Next
        }
        if kthNode == nil {
            break
        }

        // Save the next group's start
        nextGroupStart := kthNode.Next
        // Reverse the group
        newGroupStart, newGroupEnd := reverseSubList(groupStart, kthNode.Next)
        // Connect the new reversed group to the previous part
        prevGroupEnd.Next = newGroupStart
        newGroupEnd.Next = nextGroupStart
        // Move the prevGroupEnd to the end of the current reversed group
        prevGroupEnd = newGroupEnd
    }

    return dummy.Next
}
```

**Time Complexity:** O(N), where N is the number of nodes in the linked list.

**Space Complexity:** O(1) because no additional space is used other than pointers. 

---

These solutions offer both a recursive and an iterative approach to solving the problem of reversing nodes in k-groups in a linked list. Each has its advantages dependent on constraints and use cases.

