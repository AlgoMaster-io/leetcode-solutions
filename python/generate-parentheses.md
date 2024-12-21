# Leetcode Problem 22: Generate Parentheses

## Problem Link
[Leetcode 22: Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Backtracking](#approach-2-backtracking)

### Approach 1: Brute Force
The problem is to generate all combinations of well-formed parentheses given `n` pairs of parentheses.

In the brute force approach:
1. We generate all possible combinations of `n * 2` characters, i.e., sequences containing `n` opening and `n` closing parentheses.
2. For each sequence, we check if it is valid.
3. A valid sequence is one where:
   - The total number of `(` and `)` are equal.
   - At no point in the traversal of the sequence, should the number of `)` be greater than `(`.

This approach will involve generating `2^(2n)` sequences and checking the validity of each. The sequence generation can be realized with binary numbers where `0` represents `(` and `1` represents `)`.

```python
def generate_parenthesis_brute_force(n: int) -> list:
    def is_valid(sequence):
        balance = 0
        for char in sequence:
            if char == '(':
                balance += 1
            else:
                balance -= 1
            if balance < 0:
                return False
        return balance == 0

    def generate_all(current):
        if len(current) == n * 2:
            if is_valid(current):
                result.append(current)
            return
        generate_all(current + '(')
        generate_all(current + ')')

    result = []
    generate_all("")
    return result

# Time Complexity: O(2^(2n) * n) for generating all sequences and checking for validity.
# Space Complexity: O(2^(2n) * n) for storing all combinations.
```

### Approach 2: Backtracking
A more optimal way to solve the problem is using backtracking:

1. We use backtracking to recursively build the sequence.
2. We only add a `(` if it will not exceed `n`.
3. We only add a `)` if it will not make the number of closing parentheses exceed the number of opening parentheses.
4. This reduces the search space significantly since we consider only valid placement of parentheses at each step.

```python
def generate_parenthesis_backtracking(n: int) -> list:
    def backtrack(S = '', left = 0, right = 0):
        # If the current string S has reached the length of 2 * n, it's a valid combination
        if len(S) == 2 * n:
            result.append(S)
            return
        
        # If we can add a left bracket, we do so and backtrack
        if left < n:
            backtrack(S + '(', left + 1, right)

        # If we can add a right bracket, we do so and backtrack
        if right < left:
            backtrack(S + ')', left, right + 1)

    result = []
    backtrack()
    return result

# Time Complexity: O(4^n / sqrt(n)), derived from Catalan number C(n).
# Space Complexity: O(4^n / sqrt(n)), for storing all combinations due to recursion stack.
```

In this backtracking solution, we efficiently build up each combination of parentheses, ensuring at every step that the formed sequence stays valid, thus drastically reducing the number of sequences that need to be checked.

