# 902. [Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approach 1: Simple Counting with Recursive Backtracking

### Solution
csharp
```csharp
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D
public class Solution {
    public int AtMostNGivenDigitSet(string[] digits, int n) {
        string nStr = n.ToString();
        int len = nStr.Length;
        
        // Convert digit strings to character array
        char[] digitArr = new char[digits.Length];
        for (int i = 0; i < digits.Length; i++) {
            digitArr[i] = digits[i][0];
        }
        
        return Backtrack(digitArr, nStr, 0, true);
    }
    
    // Recursive function to calculate count of numbers
    private int Backtrack(char[] digits, string nStr, int index, bool limit) {
        if (index == nStr.Length) { // Base case: reached the length of nStr
            return 1;
        }
        
        int count = 0;
        foreach (char d in digits) {
            if (limit && d > nStr[index]) {
                break; // No need to consider digits greater than current nStr digit
            }
            count += Backtrack(digits, nStr, index + 1, limit && d == nStr[index]);
        }
        
        // Add numbers with length less than nStr (full set combinations)
        if (!limit) {
            int factor = 1;
            for (int i = index + 1; i < nStr.Length; i++) {
                factor *= digits.Length; // for each position, we have `|digits|` choices 
            }
            count += factor;
        }
        
        return count;
    }
}
```

## Approach 2: Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D
public class Solution {
    public int AtMostNGivenDigitSet(string[] digits, int n) {
        string nStr = n.ToString();
        int len = nStr.Length;
        int[] dp = new int[len + 1];
        dp[len] = 1;

        // Convert digit strings to character array
        char[] digitArr = new char[digits.Length];
        for (int i = 0; i < digits.Length; i++) {
            digitArr[i] = digits[i][0];
        }

        for (int i = len - 1; i >= 0; i--) {
            char c = nStr[i];
            foreach (char d in digitArr) {
                if (d < c) {
                    dp[i] += (int)Math.Pow(digits.Length, len - i - 1);
                } else if (d == c) {
                    dp[i] += dp[i + 1];
                }
            }
        }

        int result = 0;
        for (int i = 1; i < len; i++) {
            result += (int)Math.Pow(digits.Length, i);
        }
        return result + dp[0];
    }
}
```

## Approach 3: Mathematical Counting

### Solution
csharp
```csharp
// Time Complexity: O(n * m)
// Space Complexity: O(1)
// where n is the number of digits in N and m is the number of digits in D
public class Solution {
    public int AtMostNGivenDigitSet(string[] digits, int n) {
        string nStr = n.ToString();
        int len = nStr.Length;
        int[][] count = new int[len][];

        for (int i = 0; i < len; i++) {
            count[i] = new int[] { 0, 0 };
        }
        
        // Precompute the power array to avoid repetitive calculation
        int[] power = new int[len + 1];
        power[0] = 1;
        for (int i = 1; i <= len; i++) {
            power[i] = power[i - 1] * digits.Length;
        }

        for (int i = 0; i < len; i++) {
            char c = nStr[i];
            foreach (string digit in digits) {
                char d = digit[0];

                if (d < c) {
                    count[i][0] += power[len - i - 1];
                } else if (d == c) {
                    count[i][1] = 1;
                }
            }
            if (count[i][1] == 0) break;
        }

        int result = 0;
        for (int i = 0; i < len; i++) {
            result += count[i][0];
        }

        for (int i = 1; i < len; i++) {
            result += power[i];
        }

        result += count[len - 1][1];

        return result;
    }
}
```

