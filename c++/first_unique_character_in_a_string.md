# 387. [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

## Approach 1: HashMap for Character Frequency

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>
#include <string>

class Solution {
public:
    int firstUniqChar(std::string s) {
        std::unordered_map<char, int> countMap;

        // Count the frequency of each character
        for (char c : s) {
            countMap[c]++;
        }

        // Find the first character with frequency 1
        for (int i = 0; i < s.size(); i++) {
            if (countMap[s[i]] == 1) {
                return i;
            }
        }

        return -1; // No unique character found
    }
};
```

## Approach 2: Array for Character Frequency

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <string>

class Solution {
public:
    int firstUniqChar(std::string s) {
        int charCount[26] = {0}; // Array to store frequency of each character

        // Count the frequency of each character
        for (char c : s) {
            charCount[c - 'a']++;
        }

        // Find the first character with frequency 1
        for (int i = 0; i < s.size(); i++) {
            if (charCount[s[i] - 'a'] == 1) {
                return i;
            }
        }

        return -1; // No unique character found
    }
};
```

## Approach 3: Single Pass with Index Array

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <string>
#include <algorithm>
#include <climits>

class Solution {
public:
    int firstUniqChar(std::string s) {
        int index[26]; // Array to track occurrences and order of characters
        std::fill(index, index + 26, -1); // Initialize with -1 (not encountered)

        // Traverse string to fill occurrences and order
        for (int i = 0; i < s.size(); i++) {
            int charIndex = s[i] - 'a';
            if (index[charIndex] == -1) {
                index[charIndex] = i; // First occurrence
            } else {
                index[charIndex] = -2; // Mark as repeated
            }
        }

        // Find the smallest index of a unique character
        int result = INT_MAX;
        for (int i = 0; i < 26; i++) {
            if (index[i] >= 0) {
                result = std::min(result, index[i]);
            }
        }

        return result == INT_MAX ? -1 : result;
    }
};
```

