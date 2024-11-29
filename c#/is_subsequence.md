# 392. [Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Approach 1: Two Pointers (Greedy)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public bool IsSubsequence(string s, string t) {
        int sIndex = 0; // Pointer for string `s`
        int tIndex = 0; // Pointer for string `t`

        // Traverse both strings
        while (sIndex < s.Length && tIndex < t.Length) {
            if (s[sIndex] == t[tIndex]) {
                sIndex++; // Move pointer in `s` if characters match
            }
            tIndex++; // Always move pointer in `t`
        }

        // If sIndex has reached the end of `s`, it's a subsequence
        return sIndex == s.Length;
    }
}
```

## Approach 2: Iterative with Character Search

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public bool IsSubsequence(string s, string t) {
        int index = -1; // Stores the last found position of characters

        // Iterate through each character in `s`
        foreach (char c in s) {
            // Find the next occurrence of `c` in `t` after the previous index
            index = t.IndexOf(c, index + 1);
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

Use this approach when checking multiple subsequences against the same string `t`.

### Solution
csharp
```csharp
// Time Complexity: O(n + m log k) for preprocessing + query
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public bool IsSubsequence(string s, string t) {
        // Preprocess `t`: Create a map of character positions
        Dictionary<char, List<int>> charPositions = new Dictionary<char, List<int>>();
        for (int i = 0; i < t.Length; i++) {
            if (!charPositions.ContainsKey(t[i])) {
                charPositions[t[i]] = new List<int>();
            }
            charPositions[t[i]].Add(i);
        }

        int lastPosition = -1; // Tracks the position of the last matched character

        // Check each character in `s`
        foreach (char c in s) {
            if (!charPositions.ContainsKey(c)) {
                return false; // If `c` not in `t`, return false
            }

            // Perform binary search to find the next valid position
            List<int> positions = charPositions[c];
            int nextPosition = BinarySearch(positions, lastPosition + 1);
            if (nextPosition == positions.Count) {
                return false; // No valid position found
            }

            lastPosition = positions[nextPosition]; // Update lastPosition
        }

        return true;
    }

    private int BinarySearch(List<int> positions, int target) {
        int low = 0, high = positions.Count;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (positions[mid] < target) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}
```

