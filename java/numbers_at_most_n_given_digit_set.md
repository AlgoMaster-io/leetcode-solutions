# 902. [Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approach 1: Simple Counting with Recursive Backtracking

### Solution
```java
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D
public class Solution {
    public int atMostNGivenDigitSet(String[] digits, int n) {
        String nStr = String.valueOf(n);
        int len = nStr.length();
        
        // Convert digit strings to character array
        char[] digitArr = new char[digits.length];
        for (int i = 0; i < digits.length; i++) {
            digitArr[i] = digits[i].charAt(0);
        }
        
        return backtrack(digitArr, nStr, 0, true);
    }
    
    /**
     * Recursive function to calculate count of numbers
     */
    private int backtrack(char[] digits, String nStr, int index, boolean limit) {
        if (index == nStr.length()) { // Base case: reached the length of nStr
            return 1;
        }
        
        int count = 0;
        for (char d : digits) {
            if (limit && d > nStr.charAt(index)) {
                break; // No need to consider digits greater than current nStr digit
            }
            count += backtrack(digits, nStr, index + 1, limit && d == nStr.charAt(index));
        }
        
        // Add numbers with length less than nStr (full set combinations)
        if (!limit) {
            int factor = 1;
            for (int i = index + 1; i < nStr.length(); i++) {
                factor *= digits.length; // for each position, we have `|digits|` choices 
            }
            count += factor;
        }
        
        return count;
    }
}
```

## Approach 2: Dynamic Programming

### Solution
```java
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D
public class Solution {
    public int atMostNGivenDigitSet(String[] digits, int n) {
        String nStr = String.valueOf(n);
        int len = nStr.length();
        int[] dp = new int[len + 1];
        dp[len] = 1;

        // Convert digit strings to character array
        char[] digitArr = new char[digits.length];
        for (int i = 0; i < digits.length; i++) {
            digitArr[i] = digits[i].charAt(0);
        }

        for (int i = len - 1; i >= 0; i--) {
            char c = nStr.charAt(i);
            for (char d : digitArr) {
                if (d < c) {
                    dp[i] += Math.pow(digits.length, len - i - 1);
                } else if (d == c) {
                    dp[i] += dp[i + 1];
                }
            }
        }

        int result = 0;
        for (int i = 1; i < len; i++) {
            result += Math.pow(digits.length, i);
        }
        return result + dp[0];
    }
}
```

## Approach 3: Mathematical Counting

### Solution
```java
// Time Complexity: O(n * m)
// Space Complexity: O(1)
// where n is the number of digits in N and m is the number of digits in D
import java.util.Arrays;

public class Solution {
    public int atMostNGivenDigitSet(String[] digits, int n) {
        String nStr = String.valueOf(n);
        int len = nStr.length();
        int[][] count = new int[len][2];

        Arrays.setAll(count, i -> new int[]{0, 0}); 

        // Precompute the power array to avoid repetitive calculation
        int[] power = new int[len + 1];
        power[0] = 1;
        for (int i = 1; i <= len; i++) {
            power[i] = power[i - 1] * digits.length;
        }

        for (int i = 0; i < len; i++) {
            char c = nStr.charAt(i);
            for (String digit : digits) {
                char d = digit.charAt(0);

                if (d < c) {
                    count[i][0] += (power[len - i - 1]);
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

