# [Leetcode 91: Decode Ways](https://leetcode.com/problems/decode-ways/)

## Solutions
- [Approach 1: Recursive Solution (Brute Force)](#approach-1)
- [Approach 2: Recursive Solution with Memoization (Top-Down Dynamic Programming)](#approach-2)
- [Approach 3: Iterative Dynamic Programming (Bottom-Up)](#approach-3)
- [Approach 4: Optimized Iterative Dynamic Programming](#approach-4)

---

## Approach 1: Recursive Solution (Brute Force)
The problem of decoding a string of numbers is analogous to counting the number of distinct ways to break the string into valid segments (where each segment represents a valid character). Using a purely recursive approach, we can define the solution for the string based on whether the first one or two digits form a valid character.

### Intuition
- For each position in the string, we have two choices:
  1. Decode a single character if it forms a valid mapping.
  2. Decode two consecutive characters if they form a valid number between 10 and 26.
- The base case is when we've processed the entire string successfully.

### Code
```python
def numDecodings(s: str) -> int:
    def recursiveDecode(index: int) -> int:
        # Base Case: Successfully decoded the whole string
        if index == len(s):
            return 1
        # Base Case: String starts with '0' which cannot be decoded
        if s[index] == '0':
            return 0
        
        # Decode one character
        answer = recursiveDecode(index + 1)
        
        # Decode two characters
        if index + 1 < len(s) and (s[index] == '1' or (s[index] == '2' and s[index + 1] in '0123456')):
            answer += recursiveDecode(index + 2)
        
        return answer

    return recursiveDecode(0)
```

### Time and Space Complexity
- **Time Complexity:** \(O(2^n)\) due to computing each decoding possibility.
- **Space Complexity:** \(O(n)\) for the recursion call stack.

---

## Approach 2: Recursive Solution with Memoization (Top-Down Dynamic Programming)
By leveraging a memoization technique, we can store previously computed results at each index, thereby reducing the redundant calculations seen in the naive recursive approach.

### Intuition
- Use a cache (dictionary) to store the number of decodings from any position onward once it has been computed.
- This avoids repeated work and significantly speeds up the solution.

### Code
```python
def numDecodings(s: str) -> int:
    memo = {}
    
    def recursiveDecode(index: int) -> int:
        # Base Case: Successfully decoded the whole string
        if index == len(s):
            return 1
        # Base Case: String starts with '0' which cannot be decoded
        if s[index] == '0':
            return 0
        # Check memoized results
        if index in memo:
            return memo[index]

        # Decode one character
        answer = recursiveDecode(index + 1)
        
        # Decode two characters
        if index + 1 < len(s) and (s[index] == '1' or (s[index] == '2' and s[index + 1] in '0123456')):
            answer += recursiveDecode(index + 2)
        
        memo[index] = answer
        return answer

    return recursiveDecode(0)
```

### Time and Space Complexity
- **Time Complexity:** \(O(n)\) due to memoization cache hits.
- **Space Complexity:** \(O(n)\) for the memoization and recursion stack.

---

## Approach 3: Iterative Dynamic Programming (Bottom-Up)
Using a DP array, we can iteratively build the solution from the end of the string to the start.

### Intuition
- Design a DP table where `dp[i]` represents the number of ways to decode the substring from the current position `i` onward.
- Start filling the array from the end of the string backward to the start.

### Code
```python
def numDecodings(s: str) -> int:
    if not s or s[0] == '0':
        return 0
    
    n = len(s)
    dp = [0] * (n + 1)
    dp[n] = 1  # Base case for an empty substring
    
    for i in range(n - 1, -1, -1):
        if s[i] == '0':
            dp[i] = 0
        else:
            dp[i] = dp[i + 1]
            if i + 1 < n and (s[i] == '1' or (s[i] == '2' and s[i + 1] in '0123456')):
                dp[i] += dp[i + 2]
    
    return dp[0]
```

### Time and Space Complexity
- **Time Complexity:** \(O(n)\) as each character is processed once.
- **Space Complexity:** \(O(n)\) for the DP table.

---

## Approach 4: Optimized Iterative Dynamic Programming
Instead of maintaining a complete DP table, we can optimize the space usage by storing only the last two values needed at any point.

### Intuition
- Use two variables `prev1` and `prev2` to store `dp[i+1]` and `dp[i+2]`.
- This reduces the space complexity while maintaining correctness.

### Code
```python
def numDecodings(s: str) -> int:
    if not s or s[0] == '0':
        return 0
    
    n = len(s)
    prev1, prev2 = 1, 0
    
    for i in range(n - 1, -1, -1):
        current = 0
        if s[i] != '0':
            current = prev1
            if i + 1 < n and (s[i] == '1' or (s[i] == '2' and s[i + 1] in '0123456')):
                current += prev2
        
        prev2 = prev1
        prev1 = current
    
    return prev1
```

### Time and Space Complexity
- **Time Complexity:** \(O(n)\) as each character is processed once.
- **Space Complexity:** \(O(1)\) since only two variables are used to store interim results.

