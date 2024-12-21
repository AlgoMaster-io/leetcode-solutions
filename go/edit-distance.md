[Edit Distance - LeetCode Problem](https://leetcode.com/problems/edit-distance/)

## Solutions
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Recursive with Memoization](#approach-2-recursive-with-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

---

### Approach 1: Recursive Solution

**Intuition:**
The recursive solution is the most straightforward way to solve the problem by simulating the operations required to convert one string into another. For each pair of indices in the strings, we can check if the characters are the same. If they are not, we have three options: insert, delete, or replace a character and then recursively compute the cost.

#### Steps:
1. Base Case: If the length of the first string is 0, return the length of the second string (all insert operations), and vice versa.
2. If characters match, move to the next pair.
3. If they don't match, calculate the cost of insert, delete, and replace, and choose the operation with the minimum cost.

#### Code:
```go
package main

import "fmt"

func minDistanceRecursive(word1 string, word2 string) int {
    return helper(len(word1), len(word2), word1, word2)
}

func helper(len1 int, len2 int, word1 string, word2 string) int {
    if len1 == 0 {
        return len2
    }
    if len2 == 0 {
        return len1
    }
    
    if word1[len1-1] == word2[len2-1] {
        return helper(len1-1, len2-1, word1, word2)
    }
    
    insert := helper(len1, len2-1, word1, word2)
    delete := helper(len1-1, len2, word1, word2)
    replace := helper(len1-1, len2-1, word1, word2)
    
    return 1 + min(insert, delete, replace)
}

func min(a, b, c int) int {
    if a <= b && a <= c {
        return a
    }
    if b <= a && b <= c {
        return b
    }
    return c
}

func main() {
    fmt.Println(minDistanceRecursive("horse", "ros")) // Output: 3
}
```

**Complexity Analysis:**
- **Time Complexity:** O(3^(m+n)), where m and n are the lengths of word1 and word2. Each recursive call branches out three times.
- **Space Complexity:** O(m+n), due to the recursion stack.

---

### Approach 2: Recursive with Memoization

**Intuition:**
To optimize the recursive approach, we can use a memoization technique to store the results of subproblems to avoid redundant calculations.

#### Steps:
1. Maintain a 2D slice to save the results of subproblems.
2. Before computing any solution, check if it's already in the memo.
3. Proceed similarly to the recursive method otherwise.

#### Code:
```go
package main

import (
    "fmt"
)

func minDistanceMemo(word1 string, word2 string) int {
    memo := make([][]int, len(word1)+1)
    for i := range memo {
        memo[i] = make([]int, len(word2)+1)
    }
    return memoHelper(len(word1), len(word2), word1, word2, &memo)
}

func memoHelper(len1 int, len2 int, word1 string, word2 string, memo *[][]int) int {
    if len1 == 0 {
        return len2
    }
    if len2 == 0 {
        return len1
    }
    
    if (*memo)[len1][len2] != 0 {
        return (*memo)[len1][len2]
    }
    
    if word1[len1-1] == word2[len2-1] {
        (*memo)[len1][len2] = memoHelper(len1-1, len2-1, word1, word2, memo)
    } else {
        insert := memoHelper(len1, len2-1, word1, word2, memo)
        delete := memoHelper(len1-1, len2, word1, word2, memo)
        replace := memoHelper(len1-1, len2-1, word1, word2, memo)
        
        (*memo)[len1][len2] = 1 + min(insert, delete, replace)
    }
    
    return (*memo)[len1][len2]
}

func main() {
    fmt.Println(minDistanceMemo("horse", "ros")) // Output: 3
}
```

**Complexity Analysis:**
- **Time Complexity:** O(m*n), where m and n are the lengths of word1 and word2. Each subproblem is only solved once.
- **Space Complexity:** O(m*n), for storing results in the memo table.

---

### Approach 3: Dynamic Programming

**Intuition:**
The dynamic programming approach eliminates recursion and explicitly builds a table where each entry represents the minimum edit distance between substrings of the two words.

#### Steps:
1. Create a 2D DP slice initialized with sizes based on word1 and word2.
2. Initialize base cases: When one string is empty, distance equals length of the other string.
3. Compute distances iteratively for all substrings of word1 and word2.

#### Code:
```go
package main

import "fmt"

func minDistanceDP(word1 string, word2 string) int {
    dp := make([][]int, len(word1)+1)
    for i := range dp {
        dp[i] = make([]int, len(word2)+1)
    }
    
    for i := 0; i <= len(word1); i++ {
        for j := 0; j <= len(word2); j++ {
            if i == 0 {
                dp[i][j] = j // If word1 is empty
            } else if j == 0 {
                dp[i][j] = i // If word2 is empty
            } else if word1[i-1] == word2[j-1] {
                dp[i][j] = dp[i-1][j-1]
            } else {
                dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
            }
        }
    }
    
    return dp[len(word1)][len(word2)]
}

func main() {
    fmt.Println(minDistanceDP("horse", "ros")) // Output: 3
}
```

**Complexity Analysis:**
- **Time Complexity:** O(m*n), as we fill an m by n DP table.
- **Space Complexity:** O(m*n), due to the space used by the DP table.

This approach is the most optimal in terms of both time and space complexity, as it methodically builds from the simplest subproblems to solve the overall problem efficiently.

