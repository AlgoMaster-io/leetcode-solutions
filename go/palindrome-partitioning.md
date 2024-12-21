# [Leetcode 131: Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

## Approaches
1. [Backtracking Approach](#backtracking-approach)
2. [Backtracking with Dynamic Programming (Memoization)](#backtracking-with-dynamic-programming-memoization)

---

## Backtracking Approach

### Intuition
The problem is to partition a given string `s` such that every substring of the partition is a palindrome. A straightforward method is to use backtracking which tries to create partitions at every possible point and checks if the substring is a palindrome. If it is, we proceed recursively to partition the rest of the string. This approach explores all possible partitions, constructing a solution incrementally by adding one palindromic substring at a time.

### Detailed Steps
1. Use a recursive function to explore all partitions.
2. At each recursive step, check every possible substring starting from the current start index.
3. If the substring is a palindrome, add it to a list for current potential partition and recurse for the remaining string.
4. When you reach the end of the string, add the current list of substrings to the results.

### Time and Space Complexity
- **Time Complexity**: O(N * (2^N)), where N is the length of the string. The 2^N comes from the possibility of creating either a partition or not, and the N for checking if each partition is a palindrome.
- **Space Complexity**: O(N), call stack for recursion.

### Code
```go
func partition(s string) [][]string {
    var res [][]string
    var path []string
    backtrack(s, 0, &path, &res)
    return res
}

func backtrack(s string, start int, path *[]string, res *[][]string) {
    if start == len(s) {
        combination := make([]string, len(*path))
        copy(combination, *path)
        *res = append(*res, combination)
        return
    }
    
    for end := start; end < len(s); end++ {
        if isPalindrome(s, start, end) {
            *path = append(*path, s[start:end+1])
            
            // Recurse for the next potential palindrome start
            backtrack(s, end+1, path, res)
            
            // Backtrack to explore another possibility
            *path = (*path)[:len(*path)-1]
        }
    }
}

func isPalindrome(s string, start, end int) bool {
    for start < end {
        if s[start] != s[end] {
            return false
        }
        start++
        end--
    }
    return true
}
```

---

## Backtracking with Dynamic Programming (Memoization)

### Intuition
In the previous approach, the main inefficiency is repeatedly checking the same substrings for being palindromes. We can optimize this using a dynamic programming technique by storing results of palindrome checks. This reduces duplicate work and, with backtracking, leads to a more efficient solution.

### Detailed Steps
1. Use a 2D boolean array `dp` where `dp[i][j]` is true if the substring `s[i...j]` is a palindrome.
2. Initialize `dp` such that:
   - All substrings of length 1 are palindromes.
   - Substrings of length 2 are palindromes if both characters are the same.
3. For longer substrings, `s[i...j]` is a palindrome if the substring `s[i+1...j-1]` is a palindrome and `s[i] == s[j]`.
4. Use the `dp` table to optimize the palindrome checks during the backtracking process.

### Time and Space Complexity
- **Time Complexity**: O(N^2 + 2^N). Preprocessing is O(N^2) and the backtracking is O(2^N).
- **Space Complexity**: O(N^2) for the dp table.

### Code
```go
func partition(s string) [][]string {
    var res [][]string
    n := len(s)
    
    // Create a 2D dp table for storing palindrome checks
    dp := make([][]bool, n)
    for i := range dp {
        dp[i] = make([]bool, n)
    }
    
    // Fill the dp table
    for right := 0; right < n; right++ {
        for left := 0; left <= right; left++ {
            if s[left] == s[right] && (right-left < 3 || dp[left+1][right-1]) {
                dp[left][right] = true
            }
        }
    }
    
    var path []string
    backtrackWithDP(s, 0, &path, &res, dp)
    return res
}

func backtrackWithDP(s string, start int, path *[]string, res *[][]string, dp [][]bool) {
    if start == len(s) {
        combination := make([]string, len(*path))
        copy(combination, *path)
        *res = append(*res, combination)
        return
    }
    
    for end := start; end < len(s); end++ {
        if dp[start][end] {
            *path = append(*path, s[start:end+1])
            
            // Recurse for the next potential palindrome start
            backtrackWithDP(s, end+1, path, res, dp)
            
            // Backtrack to explore another possibility
            *path = (*path)[:len(*path)-1]
        }
    }
}
```

Each solution builds upon the previous one by adding optimizations such as dynamic programming to reduce unnecessary recalculations. The solutions demonstrate how backtracking can be complemented with memoization techniques to achieve better performance.

