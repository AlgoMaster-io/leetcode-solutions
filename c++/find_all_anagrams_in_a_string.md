# [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n * m), where n is the length of s and m is the length of p
// Space Complexity: O(m) for frequency array
#include <vector>
#include <string>
#include <array>

class Solution {
public:
    std::vector<int> findAnagrams(std::string s, std::string p) {
        std::vector<int> result;
        std::array<int, 26> pFreq = {0};

        // Frequency count for string p
        for (char c : p) {
            pFreq[c - 'a']++;
        }

        // Check every substring of length p in s
        for (int i = 0; i <= s.length() - p.length(); i++) {
            std::array<int, 26> sFreq = {0};

            // Count frequency of the current substring
            for (int j = i; j < i + p.length(); j++) {
                sFreq[s[j] - 'a']++;
            }

            // Compare frequencies
            if (sFreq == pFreq) {
                result.push_back(i);
            }
        }

        return result;
    }
};
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
```cpp
// Time Complexity: O(n), where n is the length of s
// Space Complexity: O(1) (fixed frequency array of size 26)
#include <vector>
#include <string>
#include <array>

class Solution {
public:
    std::vector<int> findAnagrams(std::string s, std::string p) {
        std::vector<int> result;
        std::array<int, 26> pFreq = {0};
        std::array<int, 26> sFreq = {0};

        // Frequency count for string p
        for (char c : p) {
            pFreq[c - 'a']++;
        }

        int left = 0, right = 0;

        // Sliding window over s
        while (right < s.length()) {
            // Add the current character to the window
            sFreq[s[right] - 'a']++;

            // Check if the window size matches p
            if (right - left + 1 == p.length()) {
                // Compare frequencies
                if (sFreq == pFreq) {
                    result.push_back(left);
                }

                // Remove the leftmost character from the window
                sFreq[s[left] - 'a']--;
                left++;
            }

            right++;
        }

        return result;
    }
};
```

