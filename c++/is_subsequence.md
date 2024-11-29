# 392. [Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Approach 1: Two Pointers (Greedy)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int sIndex = 0; // Pointer for string `s`
        int tIndex = 0; // Pointer for string `t`

        // Traverse both strings
        while (sIndex < s.size() && tIndex < t.size()) {
            if (s[sIndex] == t[tIndex]) {
                sIndex++; // Move pointer in `s` if characters match
            }
            tIndex++; // Always move pointer in `t`
        }

        // If sIndex has reached the end of `s`, it's a subsequence
        return sIndex == s.size();
    }
};
```

## Approach 2: Iterative with Character Search

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int index = -1; // Stores the last found position of characters

        // Iterate through each character in `s`
        for (char c : s) {
            // Find the next occurrence of `c` in `t` after the previous index
            index = t.find(c, index + 1);
            if (index == string::npos) {
                // If character not found, `s` is not a subsequence of `t`
                return false;
            }
        }

        // All characters of `s` found in `t` in order
        return true;
    }
};
```

## Approach 3: Binary Search (For Multiple Queries)

Use this approach when checking multiple subsequences against the same string t.

### Solution
```cpp
// Time Complexity: O(n + m log k) for preprocessing + query
// Space Complexity: O(n)
#include <vector>
#include <unordered_map>
#include <algorithm>

class Solution {
public:
    bool isSubsequence(string s, string t) {
        // Preprocess `t`: Create a map of character positions
        unordered_map<char, vector<int>> charPositions;
        for (int i = 0; i < t.size(); i++) {
            charPositions[t[i]].push_back(i);
        }

        int lastPosition = -1; // Tracks the position of the last matched character

        // Check each character in `s`
        for (char c : s) {
            if (charPositions.find(c) == charPositions.end()) {
                return false; // If `c` not in `t`, return false
            }

            // Perform binary search to find the next valid position
            auto& positions = charPositions[c];
            auto it = upper_bound(positions.begin(), positions.end(), lastPosition);
            if (it == positions.end()) {
                return false; // No valid position found
            }

            lastPosition = *it; // Update lastPosition
        }

        return true;
    }
};
```

