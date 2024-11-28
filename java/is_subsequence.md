# 392. [Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Approach 1: Two Pointers (Greedy)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public boolean isSubsequence(String s, String t) {
        int sIndex = 0; // Pointer for string `s`
        int tIndex = 0; // Pointer for string `t`

        // Traverse both strings
        while (sIndex < s.length() && tIndex < t.length()) {
            if (s.charAt(sIndex) == t.charAt(tIndex)) {
                sIndex++; // Move pointer in `s` if characters match
            }
            tIndex++; // Always move pointer in `t`
        }

        // If sIndex has reached the end of `s`, it's a subsequence
        return sIndex == s.length();
    }
}
```

## Approach 2: Iterative with Character Search

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public boolean isSubsequence(String s, String t) {
        int index = -1; // Stores the last found position of characters

        // Iterate through each character in `s`
        for (char c : s.toCharArray()) {
            // Find the next occurrence of `c` in `t` after the previous index
            index = t.indexOf(c, index + 1);
            if (index == -1) {
                // If character not found, `s` is not a subsequence of `t`
                return false;
            }
        }

        // All characters of `s` found in `t` in order
        return true;
    }
}
```

## Approach 3: Binary Search (For Multiple Queries)

Use this approach when checking multiple subsequences against the same string t.

### Solution
```java
// Time Complexity: O(n + m log k) for preprocessing + query
// Space Complexity: O(n)
import java.util.*;

public class Solution {
    public boolean isSubsequence(String s, String t) {
        // Preprocess `t`: Create a map of character positions
        Map<Character, List<Integer>> charPositions = new HashMap<>();
        for (int i = 0; i < t.length(); i++) {
            charPositions.computeIfAbsent(t.charAt(i), k -> new ArrayList<>()).add(i);
        }

        int lastPosition = -1; // Tracks the position of the last matched character

        // Check each character in `s`
        for (char c : s.toCharArray()) {
            if (!charPositions.containsKey(c)) {
                return false; // If `c` not in `t`, return false
            }

            // Perform binary search to find the next valid position
            List<Integer> positions = charPositions.get(c);
            int nextPosition = Collections.binarySearch(positions, lastPosition + 1);
            if (nextPosition < 0) {
                nextPosition = -(nextPosition + 1);
            }

            if (nextPosition == positions.size()) {
                return false; // No valid position found
            }

            lastPosition = positions.get(nextPosition); // Update lastPosition
        }

        return true;
    }
}
```