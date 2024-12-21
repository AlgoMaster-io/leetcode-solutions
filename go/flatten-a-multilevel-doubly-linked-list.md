# [Leetcode 430: Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)

Approaches:
- [Approach 1: Recursive DFS](#approach-1-recursive-dfs)
- [Approach 2: Iterative DFS with Stack](#approach-2-iterative-dfs-with-stack)

## Approach 1: Recursive DFS

### Intuition
The idea is to recursively traverse the multilevel list and flatten each child sublist before moving to the next node. We will do this with a helper function that returns the tail of the flattened sublist to easily connect it back to the main list.

### Code

```go
type Node struct {
    Val int
    Prev *Node
    Next *Node
    Child *Node
}

func flatten(root *Node) *Node {
    if root == nil {
        return nil
    }
    
    // Recursive function to flatten the list and return the last node
    var flattenDFS func(node *Node) *Node
    flattenDFS = func(node *Node) *Node {
        curr := node
        var last *Node
        
        for curr != nil {
            next := curr.Next
            // If current node has a child, recursively flatten it
            if curr.Child != nil {
                childLast := flattenDFS(curr.Child)
                
                // Connect current node with child
                curr.Next = curr.Child
                curr.Child.Prev = curr
                
                // If next is not nil, connect childLast to next
                if next != nil {
                    childLast.Next = next
                    next.Prev = childLast
                }
                
                curr.Child = nil // Set the child pointer to nil
                last = childLast // The last node is now the child's last node
            } else {
                last = curr // No child, last becomes current node
            }
            curr = next
        }
        return last
    }
    
    flattenDFS(root)
    return root
}
```

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the total number of nodes in the list. Each node is visited once.
- **Space Complexity**: O(n), the recursion stack could go up to depth n in the worst-case scenario where each node has one child leading to a single path downwards.

## Approach 2: Iterative DFS with Stack

### Intuition
We can use an explicit stack to simulate the recursive approach iteratively. This involves pushing nodes onto the stack when we reach a child and popping off to proceed further down the list.

### Code

```go
func flatten(root *Node) *Node {
    if root == nil {
        return nil
    }

    stack := []*Node{}
    curr := root

    for curr != nil {
        // If we encounter a child node, push the next node (if it exists) onto the stack
        if curr.Child != nil {
            if curr.Next != nil {
                stack = append(stack, curr.Next)
            }
            // Flatten the child list by directly connecting it
            curr.Next = curr.Child
            curr.Child.Prev = curr
            curr.Child = nil
        }

        // If there's no next node but we have nodes in the stack
        if curr.Next == nil && len(stack) > 0 {
            top := stack[len(stack)-1]
            stack = stack[:len(stack)-1] // Pop from stack
            curr.Next = top
            top.Prev = curr
        }

        // Move to the next node
        curr = curr.Next
    }

    return root
}
```

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the total number of nodes in the list. Each node is processed once, including potentially modifying the references.
- **Space Complexity**: O(d), where d is the maximum depth of the multilevel list. This is due to the stack storing nodes temporarily. In the worst case, d could be n/2 if every alternate node has a child.

