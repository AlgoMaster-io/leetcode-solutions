# 22. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Approach 1: Brute Force (Generate All Combinations)

### Solution
csharp
```csharp
// Time Complexity: O(2^(2n) * n), where 2^(2n) is the number of combinations and n is the time to validate each combination
// Space Complexity: O(2^(2n) * n) for storing the combinations
using System;
using System.Collections.Generic;

public class Solution {
    public IList<string> GenerateParenthesis(int n) {
        var result = new List<string>();
        GenerateAll(new char[2 * n], 0, result);
        return result;
    }

    private void GenerateAll(char[] current, int pos, List<string> result) {
        if (pos == current.Length) {
            if (IsValid(current)) {
                result.Add(new string(current)); // Add valid combination to result
            }
            return;
        }

        current[pos] = '('; // Place an open parenthesis
        GenerateAll(current, pos + 1, result);
        current[pos] = ')'; // Place a close parenthesis
        GenerateAll(current, pos + 1, result);
    }

    private bool IsValid(char[] current) {
        int balance = 0;
        foreach (char c in current) {
            if (c == '(') {
                balance++;
            } else {
                balance--;
            }
            if (balance < 0) {
                return false; // Invalid if closing parenthesis exceeds opening
            }
        }
        return balance == 0; // Valid if balance is zero
    }
}
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
csharp
```csharp
// Time Complexity: O(4^n / sqrt(n)), Catalan number
// Space Complexity: O(4^n / sqrt(n)) for storing the combinations
using System;
using System.Collections.Generic;
using System.Text;

public class Solution {
    public IList<string> GenerateParenthesis(int n) {
        var result = new List<string>();
        Backtrack(result, new StringBuilder(), 0, 0, n);
        return result;
    }

    private void Backtrack(List<string> result, StringBuilder current, int open, int close, int max) {
        if (current.Length == max * 2) {
            result.Add(current.ToString()); // Add valid combination to result
            return;
        }

        if (open < max) {
            current.Append('('); // Add an open parenthesis
            Backtrack(result, current, open + 1, close, max);
            current.Remove(current.Length - 1, 1); // Backtrack
        }
        if (close < open) {
            current.Append(')'); // Add a close parenthesis
            Backtrack(result, current, open, close + 1, max);
            current.Remove(current.Length - 1, 1); // Backtrack
        }
    }
}
```

## Approach 3: Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(4^n / sqrt(n)), Catalan number
// Space Complexity: O(4^n / sqrt(n)) for storing the combinations
using System;
using System.Collections.Generic;

public class Solution {
    public IList<string> GenerateParenthesis(int n) {
        var dp = new List<List<string>>();
        dp.Add(new List<string> { "" });

        for (int i = 1; i <= n; i++) {
            var current = new List<string>();
            for (int j = 0; j < i; j++) {
                foreach (var left in dp[j]) {
                    foreach (var right in dp[i - 1 - j]) {
                        current.Add("(" + left + ")" + right); // Generate combinations
                    }
                }
            }
            dp.Add(current);
        }

        return dp[n];
    }
}
```

