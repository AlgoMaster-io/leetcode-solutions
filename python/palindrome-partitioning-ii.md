# [Leetcode Problem 132: Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Table of Contents
- [Approach 1: Recursive Solution with Memoization](#approach-1-recursive-solution-with-memoization)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)
- [Approach 3: Optimized Dynamic Programming](#approach-3-optimized-dynamic-programming)

## Approach 1: Recursive Solution with Memoization

### Intuition
The problem of finding the minimum cuts needed to partition a string such that each substring is a palindrome can be approached recursively. The main idea is to cut the string into two parts and recursively solve the problem for each part. Memoization is used to store already computed values, which helps in reducing the number of repeated computations.

### Code
```python
def minCut(s: str) -> int:
    def isPalindrome(start, end):
        """Utility function to check if a substring s[start:end+1] is a palindrome."""
        while start < end:
            if s[start] != s[end]:
                return False
            start += 1
            end -= 1
        return True
    
    def dfs(start, memo):
        """Recursive function to find the minimum cut for substring starting at index `start`."""
        if start == len(s):  # If starting index is equal to string length, no cuts needed
            return 0
        
        if start in memo:    # Return memoized result if available
            return memo[start]
        
        min_cuts = float('inf')
        
        for end in range(start, len(s)):
            if isPalindrome(start, end):  # If substring is a palindrome
                # 1 represents a cut between start and end + 1 if 'end' is not the last character
                min_cuts = min(min_cuts, 1 + dfs(end + 1, memo))
        
        memo[start] = min_cuts
        return min_cuts
    
    # Subtract 1 since the dfs function counts the cut after the last character as well
    return dfs(0, {}) - 1
```

### Time and Space Complexity
- **Time Complexity**: O(n^2), due to checking for palindromes and memoization.
- **Space Complexity**: O(n^2) for the memoization table and recursive stack depth.

## Approach 2: Dynamic Programming

### Intuition
Using Dynamic Programming, we can improve upon the recursive approach by building up a table that directly calculates the number of cuts required. We keep track of a `dp` array where `dp[i]` represents the minimum cuts needed for the substring `s[0:i+1]`. This approach avoids recursive calls and makes use of a pre-computed palindrome table to quickly check palindrome substrings.

### Code
```python
def minCut(s: str) -> int:
    n = len(s)
    # dp[i] will be the minimum cuts needed for string s[0:i+1]
    dp = [0] * n
    # palindrome[i][j] will be True if s[i:j+1] is a palindrome
    palindrome = [[False] * n for _ in range(n)]

    for end in range(n):
        minCuts = end  # Start with a worst-case cut (cut every character)
        for start in range(end + 1):
            # A substring is a palindrome if it's of size 1 or 2 with matching borders or
            # if its inner substring is a palindrome
            if s[start] == s[end] and (end - start <= 2 or palindrome[start + 1][end - 1]):
                palindrome[start][end] = True
                # If the whole current substring is a palindrome, 0 cut needed or
                # cut at the start of the substring
                minCuts = 0 if start == 0 else min(minCuts, dp[start - 1] + 1)
        dp[end] = minCuts

    return dp[n - 1]
```

### Time and Space Complexity
- **Time Complexity**: O(n^2), due to double iteration for palindrome checking and DP table filling.
- **Space Complexity**: O(n^2) for the palindrome table, and O(n) for the dp array.

## Approach 3: Optimized Dynamic Programming

### Intuition
The previous DP approach can further be optimized in space by utilizing only a single one-dimensional DP array, keeping track of palindrome substrings more smartly.

### Code
```python
def minCut(s: str) -> int:
    n = len(s)
    dp = [0] * n
    is_palindrome = [[False] * n for _ in range(n)]

    for end in range(n):
        min_cuts = end  # Maximum cuts required is to cut each character separately
        for start in range(end + 1):
            # Check for palindrome from 'start' to 'end'
            if s[start] == s[end] and (end - start <= 2 or is_palindrome[start + 1][end - 1]):
                is_palindrome[start][end] = True
                # If start is 0 means s[0:end+1] is a palindrome, no cut needed
                min_cuts = 0 if start == 0 else min(min_cuts, dp[start - 1] + 1)
        dp[end] = min_cuts

    return dp[-1]
```

### Time and Space Complexity
- **Time Complexity**: O(n^2), iterating for palindrome checking and minimum cut computation.
- **Space Complexity**: O(n^2) due to the palindrome table, but we optimize space usage for `dp` calculation.

