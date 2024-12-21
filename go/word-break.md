# [139. Word Break](https://leetcode.com/problems/word-break/)

## Solutions
- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Recursive Backtracking with Memoization](#approach-2-recursive-backtracking-with-memoization)
- [Approach 3: Dynamic Programming (Bottom-Up)](#approach-3-dynamic-programming-bottom-up)
- [Approach 4: Breadth-First Search](#approach-4-breadth-first-search)

### Approach 1: Recursive Backtracking

The simplest way to approach this problem is via recursion. We recursively try to segment the string by checking every prefix to see if it exists in the dictionary.

#### Intuition
- Start with the entire string, check each prefix if it exists in the word dictionary.
- Recursively repeat the process for the remaining substring.
- If at any point the remaining substring is empty, it indicates successful segmentation.

#### Implementation

```go
func wordBreak(s string, wordDict []string) bool {
    return canBreak(s, wordDict, 0)
}

func canBreak(s string, wordDict []string, start int) bool {
    if start == len(s) {
        return true
    }
    for end := start + 1; end <= len(s); end++ {
        if contains(wordDict, s[start:end]) && canBreak(s, wordDict, end) {
            return true
        }
    }
    return false
}

func contains(dict []string, word string) bool {
    for _, w := range dict {
        if w == word {
            return true
        }
    }
    return false
}
```

#### Complexity
- **Time Complexity:** O(n^n) - where n is the length of the string, due to large recursion call stack in the worst case.
- **Space Complexity:** O(n) - for recursion stack.

### Approach 2: Recursive Backtracking with Memoization

Given the exponential time complexity of naive recursion, we can improve the solution using memoization. This will help us save already computed results and avoid unnecessary repeated calculations.

#### Intuition
- Store results of previous computations in a map.
- Before any recursive call, check if the result is already computed and stored in our map.

#### Implementation

```go
func wordBreak(s string, wordDict []string) bool {
    memo := make(map[int]bool)
    return canBreakMemo(s, wordDict, 0, memo)
}

func canBreakMemo(s string, wordDict []string, start int, memo map[int]bool) bool {
    if start == len(s) {
        return true
    }
    if val, ok := memo[start]; ok {
        return val
    }
    for end := start + 1; end <= len(s); end++ {
        if contains(wordDict, s[start:end]) && canBreakMemo(s, wordDict, end, memo) {
            memo[start] = true
            return true
        }
    }
    memo[start] = false
    return false
}
```

#### Complexity
- **Time Complexity:** O(n^2) - Due due to memoization, we avoid repeated work.
- **Space Complexity:** O(n) - Extra space for the recursion stack and memoization map.

### Approach 3: Dynamic Programming (Bottom-Up)

A more efficient solution is to use dynamic programming to break down the problem.

#### Intuition
- Define a boolean `dp` array where `dp[i]` represents if the substring `s[0:i]` can be segmented.
- Set `dp[0]` to true because an empty substring can be segmented trivially.
- For each substring `s[0:j]`, use previous results to determine if we can segment it.

#### Implementation

```go
func wordBreak(s string, wordDict []string) bool {
    wordSet := make(map[string]struct{})
    for _, word := range wordDict {
        wordSet[word] = struct{}{}
    }
    
    dp := make([]bool, len(s)+1)
    dp[0] = true

    for i := 1; i <= len(s); i++ {
        for j := 0; j < i; j++ {
            if dp[j] && containsMap(wordSet, s[j:i]) {
                dp[i] = true
                break
            }
        }
    }

    return dp[len(s)]
}

func containsMap(wordSet map[string]struct{}, word string) bool {
    _, exists := wordSet[word]
    return exists
}
```

#### Complexity
- **Time Complexity:** O(n^2) - Two nested loops to fill the dp array.
- **Space Complexity:** O(n) - DP array for storage.

### Approach 4: Breadth-First Search

A more pictorial approach is to use BFS. Here, each position of the string is a node, and an edge connects two positions when substring between them is in the word dictionary.

#### Intuition
- Use a queue to traverse the string level by level.
- If we manage to reach the end of the string, return true.

#### Implementation

```go
func wordBreak(s string, wordDict []string) bool {
    wordSet := make(map[string]struct{})
    for _, word := range wordDict {
        wordSet[word] = struct{}{}
    }

    queue := []int{0}
    visited := make([]bool, len(s))

    for len(queue) > 0 {
        start := queue[0]
        queue = queue[1:]

        if visited[start] {
            continue
        }
        visited[start] = true
        
        for end := start + 1; end <= len(s); end++ {
            if containsMap(wordSet, s[start:end]) {
                if end == len(s) {
                    return true
                }
                queue = append(queue, end)
            }
        }
    }

    return false
}
```

#### Complexity
- **Time Complexity:** O(n^2) - Each node can enqueue multiple times, but we ensure each index is visited once.
- **Space Complexity:** O(n) - Queue and visited array for BFS.

