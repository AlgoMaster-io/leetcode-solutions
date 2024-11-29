# 22. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Approach 1: Brute Force (Generate All Combinations)

### Solution
python
```python
# Time Complexity: O(2^(2n) * n), where 2^(2n) is the number of combinations and n is the time to validate each combination
# Space Complexity: O(2^(2n) * n) for storing the combinations

class Solution:
    def generate_parenthesis(self, n):
        result = []
        self.generate_all([''] * (2 * n), 0, result)
        return result

    def generate_all(self, current, pos, result):
        if pos == len(current):
            if self.is_valid(current):
                result.append("".join(current))  # Add valid combination to result
            return

        current[pos] = '('  # Place an open parenthesis
        self.generate_all(current, pos + 1, result)
        current[pos] = ')'  # Place a close parenthesis
        self.generate_all(current, pos + 1, result)

    def is_valid(self, current):
        balance = 0
        for c in current:
            if c == '(':
                balance += 1
            else:
                balance -= 1
            if balance < 0:
                return False  # Invalid if closing parenthesis exceeds opening
        return balance == 0  # Valid if balance is zero
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
python
```python
# Time Complexity: O(4^n / sqrt(n)), Catalan number
# Space Complexity: O(4^n / sqrt(n)) for storing the combinations

class Solution:
    def generate_parenthesis(self, n):
        result = []
        self.backtrack(result, [], 0, 0, n)
        return result

    def backtrack(self, result, current, open_count, close_count, max_pairs):
        if len(current) == max_pairs * 2:
            result.append("".join(current))  # Add valid combination to result
            return

        if open_count < max_pairs:
            current.append('(')  # Add an open parenthesis
            self.backtrack(result, current, open_count + 1, close_count, max_pairs)
            current.pop()  # Backtrack
        if close_count < open_count:
            current.append(')')  # Add a close parenthesis
            self.backtrack(result, current, open_count, close_count + 1, max_pairs)
            current.pop()  # Backtrack
```

## Approach 3: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(4^n / sqrt(n)), Catalan number
# Space Complexity: O(4^n / sqrt(n)) for storing the combinations

class Solution:
    def generate_parenthesis(self, n):
        dp = [[] for _ in range(n + 1)]
        dp[0].append("")

        for i in range(1, n + 1):
            current = []
            for j in range(i):
                for left in dp[j]:
                    for right in dp[i - 1 - j]:
                        current.append("(" + left + ")" + right)  # Generate combinations
            dp[i] = current

        return dp[n]
```

