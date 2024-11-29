# [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n * m), where n is the length of s1 and m is the length of s2
// Space Complexity: O(n)
#include <algorithm>
#include <string>

class Solution {
public:
    bool checkInclusion(std::string s1, std::string s2) {
        int n = s1.length(), m = s2.length();

        if (n > m) return false;

        // Sort s1
        std::sort(s1.begin(), s1.end());

        // Check every substring of length n in s2
        for (int i = 0; i <= m - n; i++) {
            std::string sub = s2.substr(i, n);
            std::sort(sub.begin(), sub.end());

            if (s1 == sub) {
                return true;
            }
        }

        return false;
    }
};
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
```cpp
// Time Complexity: O(m), where m is the length of s2
// Space Complexity: O(1)
#include <vector>
#include <array>

class Solution {
public:
    bool checkInclusion(std::string s1, std::string s2) {
        if (s1.length() > s2.length()) return false;

        std::array<int, 26> s1Freq = {0};
        std::array<int, 26> s2Freq = {0};

        // Frequency count for s1
        for (char c : s1) {
            s1Freq[c - 'a']++;
        }

        // Sliding window
        for (int i = 0; i < s2.length(); ++i) {
            s2Freq[s2[i] - 'a']++;

            // Remove the leftmost character from the window
            if (i >= s1.length()) {
                s2Freq[s2[i - s1.length()] - 'a']--;
            }

            // Compare the frequency arrays
            if (s1Freq == s2Freq) {
                return true;
            }
        }

        return false;
    }
};
```

