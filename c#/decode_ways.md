# 91. [Decode Ways](https://leetcode.com/problems/decode-ways/)

## Approach 1: Recursion with Memoization

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int NumDecodings(string s) {
        return DecodeHelper(s, 0, new Dictionary<int, int>());
    }
    
    private int DecodeHelper(string s, int index, Dictionary<int, int> memo) {
        // If we've reached the end of the string, return 1 for a valid path
        if (index == s.Length) {
            return 1;
        }
        
        // If the current portion of the string starts with '0', it's invalid
        if (s[index] == '0') {
            return 0;
        }
        
        // If we've already computed the result for this index, return it
        if (memo.ContainsKey(index)) {
            return memo[index];
        }
        
        // Decode one character
        int result = DecodeHelper(s, index + 1, memo);
        
        // Decode two characters if it's a valid two-digit number
        if (index + 1 < s.Length && 
            (s[index] == '1' || 
            (s[index] == '2' && s[index + 1] < '7'))) {
            result += DecodeHelper(s, index + 2, memo);
        }
        
        // Save the result to the memoization map
        memo[index] = result;
        
        return result;
    }
}
```

## Approach 2: Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
public class Solution {
    public int NumDecodings(string s) {
        if (string.IsNullOrEmpty(s) || s[0] == '0') return 0;
        
        int n = s.Length;
        int[] dp = new int[n + 1];
        
        dp[0] = 1;  // Base case: an empty string has one way to decode
        dp[1] = 1;  // Base case: the first character can't be '0'
        
        for (int i = 2; i <= n; i++) {
            int oneDigit = int.Parse(s.Substring(i - 1, 1));
            int twoDigits = int.Parse(s.Substring(i - 2, 2));
            
            if (oneDigit >= 1 && oneDigit <= 9) {
                dp[i] += dp[i - 1];
            }
            
            if (twoDigits >= 10 && twoDigits <= 26) {
                dp[i] += dp[i - 2];
            }
        }
        
        return dp[n];
    }
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int NumDecodings(string s) {
        if (string.IsNullOrEmpty(s) || s[0] == '0') return 0;

        int n = s.Length;
        int prev2 = 1;  // dp[i-2]
        int prev1 = 1;  // dp[i-1]
        
        for (int i = 1; i < n; i++) {
            int current = 0;
            int oneDigit = int.Parse(s.Substring(i, 1));
            int twoDigits = int.Parse(s.Substring(i - 1, 2));
            
            if (oneDigit >= 1 && oneDigit <= 9) {
                current += prev1;
            }
            
            if (twoDigits >= 10 && twoDigits <= 26) {
                current += prev2;
            }
            
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
}
```

