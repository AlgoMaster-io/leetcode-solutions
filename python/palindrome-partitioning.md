# [Leetcode 131: Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

## Approaches
- [Approach 1: Backtracking](#approach-1-backtracking)
- [Approach 2: Dynamic Programming with Memoization](#approach-2-dynamic-programming-with-memoization)

## Approach 1: Backtracking

### Intuition
The problem requires partitioning a string such that each substring is a palindrome. A natural approach to exploring all possible partitions is to use backtracking. We will explore every possible substring for a partition and check if it's a palindrome. If it's a palindrome, we add it to our current partition and continue to explore further. If a complete partition is achieved, we store it.

### Code
```python
def is_palindrome(s, left, right):
    # Helper function to check if a string is a palindrome
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    return True

def backtrack(start, path, result, s):
    # If we reach the end of the string, add the current partition to the result
    if start == len(s):
        result.append(path.copy())
        return
    
    # Try every possible end for the current partition
    for end in range(start, len(s)):
        if is_palindrome(s, start, end):
            # If the substring is a palindrome, add it to the path
            path.append(s[start:end+1])
            backtrack(end + 1, path, result, s)  # Explore further with the new partition
            path.pop()  # Backtrack to explore another possibility

def partition(s):
    result = []
    backtrack(0, [], result, s)
    return result
```

### Complexity Analysis
- **Time Complexity**: O(N * 2^N), where N is the length of the string. This arises because there are potentially 2^N partitions of any string, and each partition could require N time to check palindrome and copy the current path.
- **Space Complexity**: O(N), due to the space used by the recursion stack.

## Approach 2: Dynamic Programming with Memoization

### Intuition
We can optimize the backtracking approach by using a memoization technique to cache results of previously found palindrome checks. This will reduce the number of redundant palindrome calculations.

### Code
```python
def partition(s: str):
    def is_palindrome(l, r):
        # Use memorization for palindrome checks to avoid repeated work
        if (l, r) in memo:
            return memo[(l, r)]
        
        while l < r:
            if s[l] != s[r]:
                memo[(l, r)] = False
                return False
            l += 1
            r -= 1
        memo[(l, r)] = True
        return True

    def backtrack(start):
        # If we reach the end, we found a complete partition
        if start == len(s):
            result.append(path[:])  # Add a copy of the path to the result
            return
        
        for end in range(start, len(s)):
            if is_palindrome(start, end):
                path.append(s[start:end+1])  # Choose the current palindrome substring
                backtrack(end + 1)  # Explore recursive possibilities
                path.pop()  # Backtrack to explore other possibilities

    memo = {}
    result = []
    path = []
    backtrack(0)
    return result
```

### Complexity Analysis
- **Time Complexity**: O(N * 2^N). The memoization helps reduce redundant checks, but the combinatorial nature of partitioning still leads to 2^N possibilities.
- **Space Complexity**: O(N^2), due to the size of memoization table, plus O(N) for recursion stack.

