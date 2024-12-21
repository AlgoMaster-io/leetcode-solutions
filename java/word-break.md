# [Leetcode 139: Word Break](https://leetcode.com/problems/word-break/)

## Approaches
- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

---

## Approach 1: Recursion

### Intuition
In this approach, we will use recursion to break down the problem. The idea is to check if the current prefix of the string can be found in the word dictionary, and if it is, we recursively check the remaining substring. If any path returns true, we know the string can be segmented according to the dictionary.

This method may have overlapping subproblems, which makes it inefficient for larger inputs.

### Code
```java
import java.util.List;

public class WordBreak {
    public boolean wordBreak(String s, List<String> wordDict) {
        return canBreak(s, wordDict, 0);
    }

    private boolean canBreak(String s, List<String> wordDict, int start) {
        // Base case: If we are at the end of the string, it means we can segment it.
        if (start == s.length()) {
            return true;
        }
        
        // Try every possible end point for the substring.
        for (int end = start + 1; end <= s.length(); end++) {
            // Check each substring. If it exists in wordDict and further recursion returns true,
            // it means the word can be broken up properly.
            if (wordDict.contains(s.substring(start, end)) && canBreak(s, wordDict, end)) {
                return true;
            }
        }
        
        // If no breaking point results in a solution, return false.
        return false;
    }
}
```

### Complexity
- **Time Complexity**: O(2^n) where n is the length of the string `s` due to the exponential growth in possible substrings.
- **Space Complexity**: O(n) for the recursion stack (depth of the recursion tree).

---

## Approach 2: Recursion with Memoization

### Intuition
To optimize the previous recursive solution, we store the result of already processed substrings in a memoization table to avoid recomputation. This approach utilizes dynamic programming concepts to store results of subproblems, thus reducing redundant calculations.

### Code
```java
import java.util.List;
import java.util.HashMap;
import java.util.Map;

public class WordBreakMemo {
    public boolean wordBreak(String s, List<String> wordDict) {
        // Create a memoization map to store subproblem results
        Map<Integer, Boolean> memo = new HashMap<>();
        return canBreak(s, wordDict, 0, memo);
    }

    private boolean canBreak(String s, List<String> wordDict, int start, Map<Integer, Boolean> memo) {
        // If already computed, return the result from the memo.
        if (memo.containsKey(start)) {
            return memo.get(start);
        }
        
        // Check if we reached the end of the string.
        if (start == s.length()) {
            return true;
        }
        
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordDict.contains(s.substring(start, end)) && canBreak(s, wordDict, end, memo)) {
                memo.put(start, true);
                return true;
            }
        }
        
        // Memoize the result for the current starting index.
        memo.put(start, false);
        return false;
    }
}
```

### Complexity
- **Time Complexity**: O(n^2) for each substring and position due to memoization.
- **Space Complexity**: O(n) for the memoization table.

---

## Approach 3: Dynamic Programming

### Intuition
We can further improve the solution by using dynamic programming. The idea is to use a boolean dp array where `dp[i]` represents whether the substring `s[0...i]` can be segmented using the dictionary. For each position, we check all possible partitions, updating the dp array based on previously computed results.

### Code
```java
import java.util.List;
import java.util.HashSet;
import java.util.Set;

public class WordBreakDP {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordSet = new HashSet<>(wordDict); // Convert list to set for O(1) look-up times
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true; // Base case: an empty string is "breakable"
        
        // Check each possible end-point for substrings.
        for (int end = 1; end <= s.length(); end++) {
            for (int start = 0; start < end; start++) {
                // If the start point is breakable and the substring s[start:end] is in the dictionary
                if (dp[start] && wordSet.contains(s.substring(start, end))) {
                    dp[end] = true;
                    break; // No need to check further substrings if we found one that works
                }
            }
        }
        
        return dp[s.length()]; // The result for the whole string is in dp[n]
    }
}
```

### Complexity
- **Time Complexity**: O(n^3) because of the two nested loops and substring check, but efficiently optimized by using a set.
- **Space Complexity**: O(n) for the dp array.

---

Each solution builds on top of the previous, gradually introducing optimizations to tackle the problem more efficiently. The Dynamic Programming approach is the most efficient and is typically used in practice for problems like these.

