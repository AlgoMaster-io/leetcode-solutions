# [Leetcode 91: Decode Ways](https://leetcode.com/problems/decode-ways/)

In this problem, you are asked to determine the total number of ways to decode a given string that consists of digits only. Each digit can be mapped to a letter using the mapping 1 -> 'A', 2 -> 'B', ..., 26 -> 'Z'. 

## Approaches:

1. [Recursive Approach](#recursive-approach)
2. [Memoization (Top-Down Dynamic Programming)](#memoization-top-down-dynamic-programming)
3. [Dynamic Programming (Bottom-Up)](#dynamic-programming-bottom-up)
4. [Space Optimized Dynamic Programming](#space-optimized-dynamic-programming)

---

### Recursive Approach

This approach uses recursion to explore all possible paths. For each position in the string, we can decode as a single digit or a pair of digits (if valid).

#### Intuition:
- For each character, there are two decisions: decode it as a single character or as a pair.
- We recursively calculate the total number of ways by making these choices.

```csharp
public class Solution {
    public int NumDecodings(string s) {
        if (string.IsNullOrEmpty(s)) return 0;
        return Decode(s, 0);
    }

    private int Decode(string s, int index) {
        // Base case: when you reach the end of the string
        if (index == s.Length) return 1;

        // If the current position holds a zero, it's invalid
        if (s[index] == '0') return 0;

        // Decode single digit
        int result = Decode(s, index + 1);

        // Decode two digits if valid
        if (index < s.Length - 1 && (s[index] == '1' || (s[index] == '2' && "0123456".Contains(s[index + 1])))) {
            result += Decode(s, index + 2);
        }

        return result;
    }
}
```

- **Time Complexity**: O(2^n) due to possible overlapping subproblems
- **Space Complexity**: O(n) due to recursion stack

---

### Memoization (Top-Down Dynamic Programming)

This approach optimizes the recursive solution by storing the results of subproblems to avoid recomputation.

#### Intuition:
- Similar exploration as recursion, but we store results in a dictionary to avoid repeated work.

```csharp
public class Solution {
    public int NumDecodings(string s) {
        if (string.IsNullOrEmpty(s)) return 0;
        Dictionary<int, int> memo = new Dictionary<int, int>();
        return Decode(s, 0, memo);
    }

    private int Decode(string s, int index, Dictionary<int, int> memo) {
        if (index == s.Length) return 1;
        if (s[index] == '0') return 0;
        
        if (memo.ContainsKey(index)) return memo[index];

        int result = Decode(s, index + 1, memo);

        if (index < s.Length - 1 && (s[index] == '1' || (s[index] == '2' && "0123456".Contains(s[index + 1])))) {
            result += Decode(s, index + 2, memo);
        }

        memo[index] = result;
        return result;
    }
}
```

- **Time Complexity**: O(n) since each index is computed once
- **Space Complexity**: O(n) for the recursion stack and memo dictionary

---

### Dynamic Programming (Bottom-Up)

This iterative approach builds the solution using a DP array where each entry represents the number of ways to decode up to that index.

#### Intuition:
- Start from the beginning and iteratively compute the number of decoding ways using previously computed values.

```csharp
public class Solution {
    public int NumDecodings(string s) {
        if (string.IsNullOrEmpty(s) || s[0] == '0') return 0;
        
        int n = s.Length;
        int[] dp = new int[n + 1];
        dp[0] = 1; // Base case, empty string

        for (int i = 1; i <= n; i++) {
            // Single digit
            if (s[i - 1] != '0') {
                dp[i] += dp[i - 1];
            }
            
            // Two digits
            if (i > 1 && (s[i - 2] == '1' || (s[i - 2] == '2' && s[i - 1] >= '0' && s[i - 1] <= '6'))) {
                dp[i] += dp[i - 2];
            }
        }
        
        return dp[n];
    }
}
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(n) for the dp array

---

### Space Optimized Dynamic Programming

This approach improves space efficiency by only keeping the last two states instead of the whole array.

#### Intuition:
- We only need the last two results to calculate the current result, so keep track of them using two variables.

```csharp
public class Solution {
    public int NumDecodings(string s) {
        if (string.IsNullOrEmpty(s) || s[0] == '0') return 0;
        
        int prev2 = 1, prev1 = 1; // dp[i-2] and dp[i-1] initializes
        
        for (int i = 1; i < s.Length; i++) {
            int current = 0;
            if (s[i] != '0') {
                current += prev1;
            }
            
            if (s[i-1] == '1' || (s[i-1] == '2' && s[i] >= '0' && s[i] <= '6')) {
                current += prev2;
            }
            
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
}
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(1) - only a constant amount of space is used regardless of the input size.

These solutions lead from the most basic recursive way to an optimal use of dynamic programming with space optimization, demonstrating a clear path of optimization.

