# 14. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Approach 1: Horizontal Scanning

### Solution
```cpp
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(1)
#include <string>
#include <vector>

class Solution {
public:
    std::string longestCommonPrefix(std::vector<std::string>& strs) {
        if (strs.empty()) {
            return "";
        }

        // Start with the first string as the prefix
        std::string prefix = strs[0];

        // Compare the prefix with each string in the array
        for (int i = 1; i < strs.size(); i++) {
            while (strs[i].find(prefix) != 0) {
                // Shorten the prefix until it matches the start of the current string
                prefix = prefix.substr(0, prefix.size() - 1);
                if (prefix.empty()) {
                    return ""; // No common prefix
                }
            }
        }

        return prefix;
    }
};
```

## Approach 2: Vertical Scanning

### Solution
```cpp
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(1)
#include <string>
#include <vector>

class Solution {
public:
    std::string longestCommonPrefix(std::vector<std::string>& strs) {
        if (strs.empty()) {
            return "";
        }

        // Iterate over each character of the first string
        for (int i = 0; i < strs[0].length(); i++) {
            char c = strs[0][i];

            // Compare this character with the same position in all strings
            for (int j = 1; j < strs.size(); j++) {
                // If characters don't match or exceed the string's length
                if (i >= strs[j].length() || strs[j][i] != c) {
                    return strs[0].substr(0, i); // Return the common prefix
                }
            }
        }

        return strs[0]; // The entire first string is the common prefix
    }
};
```

## Approach 3: Divide and Conquer

### Solution
```cpp
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(m * log n) where m is the average string length
#include <string>
#include <vector>

class Solution {
public:
    std::string longestCommonPrefix(std::vector<std::string>& strs) {
        if (strs.empty()) {
            return "";
        }

        return longestCommonPrefix(strs, 0, strs.size() - 1);
    }

private:
    std::string longestCommonPrefix(const std::vector<std::string>& strs, int left, int right) {
        if (left == right) {
            return strs[left]; // Base case: single string
        }

        // Divide the array into two halves
        int mid = (left + right) / 2;
        std::string lcpLeft = longestCommonPrefix(strs, left, mid);
        std::string lcpRight = longestCommonPrefix(strs, mid + 1, right);

        // Merge results
        return commonPrefix(lcpLeft, lcpRight);
    }

    std::string commonPrefix(const std::string& left, const std::string& right) {
        int minLength = std::min(left.length(), right.length());
        for (int i = 0; i < minLength; i++) {
            if (left[i] != right[i]) {
                return left.substr(0, i);
            }
        }
        return left.substr(0, minLength);
    }
};
```

## Approach 4: Binary Search

### Solution
```cpp
// Time Complexity: O(S * log m) where S is the sum of characters in strings and m is the minimum string length
// Space Complexity: O(1)
#include <string>
#include <vector>

class Solution {
public:
    std::string longestCommonPrefix(std::vector<std::string>& strs) {
        if (strs.empty()) {
            return "";
        }

        int minLen = INT_MAX;
        for (const std::string& str : strs) {
            minLen = std::min(minLen, int(str.length()));
        }

        int low = 1, high = minLen;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (isCommonPrefix(strs, mid)) {
                low = mid + 1; // Try for a longer prefix
            } else {
                high = mid - 1; // Try for a shorter prefix
            }
        }

        return strs[0].substr(0, (low + high) / 2);
    }

private:
    bool isCommonPrefix(const std::vector<std::string>& strs, int len) {
        std::string prefix = strs[0].substr(0, len);
        for (int i = 1; i < strs.size(); i++) {
            if (strs[i].substr(0, len) != prefix) {
                return false;
            }
        }
        return true;
    }
};
```

