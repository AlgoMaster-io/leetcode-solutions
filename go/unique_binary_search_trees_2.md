# 95. [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Approach 1: Recursive Generation

### Solution
```go
// Time Complexity: Catalan number time complexity (approximately O(4^n / n^(3/2)))
// Space Complexity: Catalan space complexity due to recursion stack
package main

import "fmt"

// TreeNode structure
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func generateTrees(n int) []*TreeNode {
    if n == 0 {
        return []*TreeNode{}
    }
    return generateTreesHelper(1, n)
}

func generateTreesHelper(start, end int) []*TreeNode {
    var allTrees []*TreeNode
    if start > end {
        allTrees = append(allTrees, nil)
        return allTrees
    }

    // Iterate through each number as the root
    for i := start; i <= end; i++ {
        // Generate all left and right subtrees
        leftTrees := generateTreesHelper(start, i-1)
        rightTrees := generateTreesHelper(i+1, end)

        // Combine left and right subtrees with the root i
        for _, left := range leftTrees {
            for _, right := range rightTrees {
                currentTree := &TreeNode{Val: i}
                currentTree.Left = left
                currentTree.Right = right
                allTrees = append(allTrees, currentTree)
            }
        }
    }
    return allTrees
}

func main() {
    results := generateTrees(3)
    for _, res := range results {
        fmt.Println(res)
    }
}
```

## Approach 2: Dynamic Programming (Memoization) - Optimized Recursive

### Solution
```go
// Time Complexity: Catalan number time complexity
// Space Complexity: O(n^2) for the memoization table
package main

import (
    "fmt"
    "strconv"
)

// TreeNode structure
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

type Solution struct {
    memo map[string][]*TreeNode
}

func generateTrees(n int) []*TreeNode {
    if n == 0 {
        return []*TreeNode{}
    }
    solution := Solution{memo: make(map[string][]*TreeNode)}
    return solution.generateTreesHelper(1, n)
}

func (s *Solution) generateTreesHelper(start, end int) []*TreeNode {
    var allTrees []*TreeNode
    if start > end {
        allTrees = append(allTrees, nil)
        return allTrees
    }

    memoKey := strconv.Itoa(start) + "-" + strconv.Itoa(end)
    if val, exists := s.memo[memoKey]; exists {
        return val
    }

    // Iterate through each number as the root
    for i := start; i <= end; i++ {
        // Generate all left and right subtrees
        leftTrees := s.generateTreesHelper(start, i-1)
        rightTrees := s.generateTreesHelper(i+1, end)

        // Combine left and right subtrees with the root i
        for _, left := range leftTrees {
            for _, right := range rightTrees {
                currentTree := &TreeNode{Val: i}
                currentTree.Left = left
                currentTree.Right = right
                allTrees = append(allTrees, currentTree)
            }
        }
    }

    s.memo[memoKey] = allTrees
    return allTrees
}

func main() {
    results := generateTrees(3)
    for _, res := range results {
        fmt.Println(res)
    }
}
```

The recursive approach builds trees by choosing different nodes as roots and generating all possible left and right subtrees. The memoization approach improves efficiency by storing already calculated results to avoid redundant calculations.

