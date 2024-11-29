# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <unordered_set>
#include <algorithm>

class Solution {
public:
    int lengthOfLongestSubstring(std::string s) {
        int maxLength = 0;

        // Check every possible substring
        for (int i = 0; i < s.length(); i++) {
            std::unordered_set<char> seen;
            int length = 0;

            for (int j = i; j < s.length(); j++) {
                if (seen.find(s[j]) != seen.end()) {
                    break; // Break if a duplicate character is found
                }
                seen.insert(s[j]);
                length++;
                maxLength = std::max(maxLength, length); // Update maxLength
            }
        }

        return maxLength;
    }
};
```

## Approach 2: Sliding Window with HashSet

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_set>
#include <algorithm>

class Solution {
public:
    int lengthOfLongestSubstring(std::string s) {
        std::unordered_set<char> set;
        int maxLength = 0;
        int left = 0;

        // Sliding window
        for (int right = 0; right < s.length(); right++) {
            while (set.find(s[right]) != set.end()) {
                set.erase(s[left]); // Shrink the window
                left++;
            }
            set.insert(s[right]); // Expand the window
            maxLength = std::max(maxLength, right - left + 1); // Update maxLength
        }

        return maxLength;
    }
};
```

## Approach 3: Sliding Window with HashMap (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>
#include <algorithm>

class Solution {
public:
    int lengthOfLongestSubstring(std::string s) {
        std::unordered_map<char, int> map;
        int maxLength = 0;
        int left = 0;

        // Sliding window
        for (int right = 0; right < s.length(); right++) {
            char currentChar = s[right];

            // If the character is already in the map, update the left pointer
            if (map.find(currentChar) != map.end()) {
                left = std::max(left, map[currentChar] + 1);
            }

            // Update the character's last index in the map
            map[currentChar] = right;

            // Update maxLength
            maxLength = std::max(maxLength, right - left + 1);
        }

        return maxLength;
    }
};
```

