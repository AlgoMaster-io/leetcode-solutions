# [LeetCode 91: Decode Ways](https://leetcode.com/problems/decode-ways/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)

### Recursive Approach

The recursive approach attempts to solve the problem by considering each character individually and tries to decode either one or two characters at a time. This explores all possible combinations leading to a solution.

**Intuition:**
- If we encounter a single character from '1' to '9', it can be decoded to a letter from 'A' to 'I'.
- A pair of characters from '10' to '26' can be decoded into a letter from 'J' to 'Z'.
- We return 0 if the current character is '0' because it doesn't map to any letter directly.
- For each character, recursively calculate the number of ways to decode the subsequent string.

```java
public class DecodeWaysRecursive {
    public int numDecodings(String s) {
        return decode(s, 0);
    }

    private int decode(String s, int index) {
        if (index == s.length()) {
            return 1; // Reached the end successfully
        }
        if (s.charAt(index) == '0') {
            return 0; // '0' can't be decoded
        }

        // Take one character and move to the next
        int ways = decode(s, index + 1);
        
        // Take two characters and ensure it's valid from '10' to '26'
        if (index + 1 < s.length() && 
            (s.charAt(index) == '1' || (s.charAt(index) == '2' && s.charAt(index + 1) <= '6'))) {
            ways += decode(s, index + 2);
        }
        
        return ways;
    }
}
```

**Time Complexity:** \(O(2^N)\), where \(N\) is the length of the string due to the branching at each step.

**Space Complexity:** \(O(N)\) due to the recursion stack.

### Memoization Approach

The memoization approach is an optimization over the recursive approach where we store the results of subproblems to avoid redundant calculations.

**Intuition:**
- We use an array to store results of previously computed indices, avoiding recalculations and reducing time complexity.
- Similar recursive structure, but check the memoized results before computing.

```java
import java.util.HashMap;
import java.util.Map;

public class DecodeWaysMemoization {
    private Map<Integer, Integer> memo = new HashMap<>();

    public int numDecodings(String s) {
        return decode(s, 0);
    }

    private int decode(String s, int index) {
        if (index == s.length()) {
            return 1; // Reached the end successfully
        }
        if (s.charAt(index) == '0') {
            return 0; // '0' can't be decoded
        }
        if (memo.containsKey(index)) {
            return memo.get(index);
        }

        int ways = decode(s, index + 1);
        if (index + 1 < s.length() &&
            (s.charAt(index) == '1' || (s.charAt(index) == '2' && s.charAt(index + 1) <= '6'))) {
            ways += decode(s, index + 2);
        }
        
        memo.put(index, ways);
        return ways;
    }
}
```

**Time Complexity:** \(O(N)\), due to storing results and avoiding recomputation.

**Space Complexity:** \(O(N)\), both for the recursion stack and memoization map.

### Dynamic Programming Approach

The DP approach builds from the bottom up, filling an array where each position represents the number of ways to decode up to that point.

**Intuition:**
- Use a `dp` array where `dp[i]` stores the count of decode ways up to index `i`.
- Initialize `dp[0]` to `1` as the base case (empty string).
- Iterate through the string, filling `dp` based on allowable single and double character decodings.

```java
public class DecodeWaysDP {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0 || s.charAt(0) == '0') return 0;

        int n = s.length();
        int[] dp = new int[n + 1];
        dp[0] = 1; // Base case: an empty string has one decode way
        
        for (int i = 1; i <= n; i++) {
            // Can the current single digit be decoded?
            if (s.charAt(i - 1) != '0') {
                dp[i] += dp[i - 1];
            }

            // Can the two-character string be decoded?
            if (i > 1 && (s.charAt(i - 2) == '1' || 
                         (s.charAt(i - 2) == '2' && s.charAt(i - 1) <= '6'))) {
                dp[i] += dp[i - 2];
            }
        }
        
        return dp[n];
    }
}
```

**Time Complexity:** \(O(N)\), iterating through the string once.

**Space Complexity:** \(O(N)\), for the `dp` array. 

This DP approach optimizes our need to extensively traverse parts of the string by pre-computing results in a structured manner.

