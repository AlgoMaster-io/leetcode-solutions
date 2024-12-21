# [Leetcode 32: Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Stack](#approach-2-stack)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)
- [Approach 4: Two-Pointer Technique](#approach-4-two-pointer-technique)

---

## Approach 1: Brute Force

### Intuition
This naive approach involves generating all possible substrings of the input string and checking each one to see if it is a valid parentheses substring. This method helps in understanding and validating smaller examples but is not efficient for larger inputs.

### Code
```python
def longestValidParentheses(s: str) -> int:
    def is_valid(substring):
        # Check if the given substring is a valid parentheses sequence
        balance = 0
        for char in substring:
            if char == '(':
                balance += 1
            else:
                balance -= 1
            # If balance goes negative, it's invalid
            if balance < 0:
                return False
        return balance == 0

    n = len(s)
    max_length = 0
    # Iterate through all possible substrings
    for i in range(n):
        for j in range(i + 2, n + 1, 2):  # Consider only even length substrings
            if is_valid(s[i:j]):
                max_length = max(max_length, j - i)
    return max_length
```

### Time Complexity
- **O(n^3):** Due to the nested loops and substring validation, this approach ends up with cubic time complexity.

### Space Complexity
- **O(1):** The space used by the algorithm is constant.

---

## Approach 2: Stack

### Intuition
Using a stack helps to track the indices of parentheses. The idea is to store the index of the last unmatched ')'. For every valid '()', we know it matches with an earlier unmatched '(', allowing the calculation of the longest valid parentheses substring length.

### Code
```python
def longestValidParentheses(s: str) -> int:
    stack = [-1]
    max_length = 0
    for i, char in enumerate(s):
        if char == '(':
            stack.append(i)
        else:
            stack.pop()
            if not stack:
                stack.append(i)
            else:
                max_length = max(max_length, i - stack[-1])
    return max_length
```

### Time Complexity
- **O(n):** We iterate through the string once.

### Space Complexity
- **O(n):** In the worst case, the stack can store all indices.

---

## Approach 3: Dynamic Programming

### Intuition
Dynamic programming can be used by utilizing an array `dp` where `dp[i]` represents the length of the longest valid substring ending at index `i`. By examining patterns in valid parentheses sequences and their relationship with prior indices, we can build solutions to progressively larger subproblems.

### Code
```python
def longestValidParentheses(s: str) -> int:
    n = len(s)
    if n <= 1:
        return 0
    dp = [0] * n
    max_length = 0
    for i in range(1, n):
        if s[i] == ')':
            if s[i - 1] == '(':
                dp[i] = dp[i - 2] + 2 if i >= 2 else 2
            elif i - dp[i - 1] > 0 and s[i - dp[i - 1] - 1] == '(':
                dp[i] = dp[i - 1] + 2 + (dp[i - dp[i - 1] - 2] if (i - dp[i - 1]) >= 2 else 0)
            max_length = max(max_length, dp[i])
    return max_length
```

### Time Complexity
- **O(n):** We iterate through the string once.

### Space Complexity
- **O(n):** `dp` array holds results for each index.

---

## Approach 4: Two-Pointer Technique

### Intuition
This method involves two passes over the string. The first pass goes from left to right, counting '(' and ')'. If the counts are equal, update the max length. If the count of ')' exceeds '(', reset counts. The second pass goes from right to left, applying similar logic, to ensure that unmatched '(' before valid `)` are handled.

### Code
```python
def longestValidParentheses(s: str) -> int:
    left = right = max_length = 0
    n = len(s)

    # Left to right pass
    for i in range(n):
        if s[i] == '(':
            left += 1
        else:
            right += 1
        if left == right:
            max_length = max(max_length, 2 * right)
        elif right > left:
            left = right = 0

    left = right = 0
    # Right to left pass
    for i in range(n - 1, -1, -1):
        if s[i] == ')':
            right += 1
        else:
            left += 1
        if left == right:
            max_length = max(max_length, 2 * left)
        elif left > right:
            left = right = 0

    return max_length
```

### Time Complexity
- **O(n):** Two passes over the string, each linear.

### Space Complexity
- **O(1):** Only using a fixed amount of additional space.

