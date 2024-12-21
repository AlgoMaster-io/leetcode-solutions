# [132. Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approaches
- [Brute Force - Recursive approach with memoization](#brute-force---recursive-approach-with-memoization)
- [Dynamic Programming](#dynamic-programming-approach)

---

### Brute Force - Recursive Approach with Memoization

**Intuition**:
To solve this problem, a naive approach would be to recursively check all possible partitions of the input string and count the minimum number of cuts required to partition it into substrings that are all palindromes. However, this can be highly inefficient due to overlapping subproblems. Hence, we can enhance this approach by memoizing already computed results to avoid recalculating results for the same subproblems.

```go
func minCut(s string) int {
    n := len(s)
    memo := make(map[int]int)
    
    var isPalindrome func(l, r int) bool
    isPalindrome = func(l, r int) bool {
        for l < r {
            if s[l] != s[r] {
                return false
            }
            l++
            r--
        }
        return true
    }
    
    var minCutsHelper func(start int) int
    minCutsHelper = func(start int) int {
        if start == n {
            return 0
        }
        if _, found := memo[start]; found {
            return memo[start]
        }
        
        minCuts := int(^uint(0) >> 1) // equivalent to 'Inf' or max int

        // Try every possible partition from 'start'
        for end := start; end < n; end++ {
            if isPalindrome(start, end) {
                cuts := 1 + minCutsHelper(end + 1)
                if cuts < minCuts {
                    minCuts = cuts
                }
            }
        }
        
        memo[start] = minCuts
        return minCuts
    }
    
    return minCutsHelper(0) - 1
}
```

**Time Complexity**: O(n^2), where n is the length of the string. Each substring is checked multiple times.

**Space Complexity**: O(n), due to the recursion stack and memoization usage.

---

### Dynamic Programming Approach

**Intuition**:
To optimize further, a dynamic programming approach is used. We maintain a DP table `dp[i]` where `dp[i]` represents the minimum cuts needed for a palindrome partitioning of the substring `s[0:i+1]`. An auxiliary table is used to store whether a substring `s[j:i]` is a palindrome. 

```go
func minCut(s string) int {
    n := len(s)
    // pal[i][j] will be true if s[i:j+1] is a palindrome
    pal := make([][]bool, n)
    for i := range pal {
        pal[i] = make([]bool, n)
    }
    
    dp := make([]int, n)
    for i := range dp {
        dp[i] = i // initialize with the maximum number of cuts
    }
    
    for end := 0; end < n; end++ {
        for start := 0; start <= end; start++ {
            if s[start] == s[end] && (end-start <= 1 || pal[start+1][end-1]) {
                pal[start][end] = true
                // If pal is palindrome, update dp[end]
                if start == 0 {
                    dp[end] = 0
                } else {
                    dp[end] = min(dp[end], dp[start-1] + 1)
                }
            }
        }
    }
    
    return dp[n-1]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

**Time Complexity**: O(n^2). We iterate over each substring to determine if it is a palindrome, and we make O(1) operations for each pair.

**Space Complexity**: O(n^2). The space is due to the 2D array used to store palindrome information and the dp array.

---

Each approach builds upon the previous, leading to a more efficient solution with reduced time complexity by applying dynamic programming techniques.

