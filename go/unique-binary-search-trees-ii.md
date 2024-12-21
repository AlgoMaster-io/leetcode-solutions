# [Leetcode 95: Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

- [Approach 1: Recursive Approach with Catalan Number Theory](#approach-1-recursive-approach-with-catalan-number-theory)
- [Approach 2: Dynamic Programming with Memoization](#approach-2-dynamic-programming-with-memoization)

## Approach 1: Recursive Approach with Catalan Number Theory

### Intuition

This problem can be understood through the lens of Catalan numbers. A catalan number describes the number of unique binary search trees (BSTs) that can be created with `n` distinct values. The recursive approach involves generating all possible BSTs for every possible root node and then recursively generating left and right subtrees for each root possibility. We combine all possible left and right subtrees to form unique BST combinations.

### Detailed Steps

1. Base Case: If `n == 0`, return an empty list since there are no BSTs possible with zero nodes.
2. Use a recursive function to construct the tree:
   - For a given range of numbers, choose each number as a potential root.
   - Recursively generate all left subtrees from the subset of numbers that form the left child.
   - Recursively generate all right subtrees from the subset of numbers that form the right child.
   - Combine each left subtree with each right subtree and current root to form all possible trees.

### Time and Space Complexity

- **Time Complexity**: O(n * C_n), where C_n is the nth Catalan number. This is because we generate all the unique BSTs.
- **Space Complexity**: O(n * C_n), for the recursive call stack and result storage of C_n trees each with n nodes at worst.

### Code

```go
package main

import (
    "fmt"
)

// TreeNode represents a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

// generateTrees generates all unique BSTs for values 1 to n.
func generateTrees(n int) []*TreeNode {
    if n == 0 {
        return nil
    }
    return generate(1, n)
}

// generate generates all BSTs using tree values from start to end.
func generate(start, end int) []*TreeNode {
    if start > end {
        return []*TreeNode{nil} // Base case: single tree which is empty
    }

    var allTrees []*TreeNode
    // Try each number as a root.
    for i := start; i <= end; i++ {
        // Recursive case: generate all left and right subtrees.
        leftTrees := generate(start, i-1)  // All numbers < i form left subtrees
        rightTrees := generate(i+1, end)   // All numbers > i form right subtrees

        // Connect left and right subtrees to the root.
        for _, l := range leftTrees {
            for _, r := range rightTrees {
                currentTree := &TreeNode{Val: i}
                currentTree.Left = l
                currentTree.Right = r
                allTrees = append(allTrees, currentTree)
            }
        }
    }
    return allTrees
}


func main() {
    trees := generateTrees(3)
    fmt.Println(len(trees)) // Outputs: 5
}
```

## Approach 2: Dynamic Programming with Memoization

### Intuition

Instead of calculating the result from scratch each time for overlapping subproblems, we employ memoization to store previously calculated results of subtree formations. This optimized dynamic programming approach reduces redundant calculations by storing the results of subproblems already solved.

### Detailed Steps

1. We use an auxiliary function to generate all BSTs for a given integer range [start, end].
2. Use memoization: store results of subproblems (i.e., the tuple (start, end)) to avoid recomputation.
3. The combination logic remains similar – choosing each number in the range as the root and recursively building left and right subtrees.

### Time and Space Complexity

- **Time Complexity**: O(n^2 * C_n), which improves over the pure recursive approach by eliminating redundant calculations.
- **Space Complexity**: O(n^2 * C_n), due to memoization storage and tree construction.

### Code

```go
package main

import (
    "fmt"
)

// TreeNode represents a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

// Memoization map to store computed subproblem solutions.
var memo map[[2]int][]*TreeNode

// generateTreesMemo generates all unique BSTs for values 1 to n using memoization.
func generateTreesMemo(n int) []*TreeNode {
    memo = make(map[[2]int][]*TreeNode)
    if n == 0 {
        return nil
    }
    return generateWithMemo(1, n)
}

// generateWithMemo generates all BSTs using tree values from start to end using memoization.
func generateWithMemo(start, end int) []*TreeNode {
    if start > end {
        return []*TreeNode{nil}
    }

    // Check the memoization map for prior calculations.
    if result, found := memo[[2]int{start, end}]; found {
        return result
    }

    var allTrees []*TreeNode
    for i := start; i <= end; i++ {
        leftTrees := generateWithMemo(start, i-1)
        rightTrees := generateWithMemo(i+1, end)

        for _, l := range leftTrees {
            for _, r := range rightTrees {
                currentTree := &TreeNode{Val: i}
                currentTree.Left = l
                currentTree.Right = r
                allTrees = append(allTrees, currentTree)
            }
        }
    }

    // Store the result in the memoization map.
    memo[[2]int{start, end}] = allTrees
    return allTrees
}

func main() {
    trees := generateTreesMemo(3)
    fmt.Println(len(trees)) // Outputs: 5
}
```

These approaches provide you with a comprehensive understanding of generating unique binary search trees using combinatorial mathematics and dynamic programming with memoization in Go.

