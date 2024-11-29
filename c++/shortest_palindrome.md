# 214. [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approach 1: Brute Force Reverse and Check

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <string>
#include <algorithm>

class Solution {
public:
    std::string shortestPalindrome(std::string s) {
        // Edge case for empty string
        if (s.empty()) return s;

        // Reverse the string
        std::string sb = s;
        std::reverse(sb.begin(), sb.end());

        // Try to find a palindrome by checking each position
        for (int i = 0; i < s.length(); i++) {
            // Check if the substring of s and reversed s is palindrome
            if (s.substr(0) == sb.substr(i)) {
                return sb.substr(0, i) + s;
            }
        }

        return ""; // This will never be executed
    }
};
```

## Approach 2: KMP Algorithm

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <string>
#include <algorithm>

class Solution {
public:
    std::string shortestPalindrome(std::string s) {
        std::string rev = s;
        std::reverse(rev.begin(), rev.end());
        std::string combined = s + "#" + rev;

        // Compute KMP table
        std::vector<int> kmpTable = computeKMPTable(combined);

        // The palindrome length from using kmpTable
        int palindromeLength = kmpTable.back();

        // Append the part missing to form a palindrome
        return rev.substr(0, rev.length() - palindromeLength) + s;
    }

private:
    std::vector<int> computeKMPTable(const std::string& s) {
        std::vector<int> kmpTable(s.length(), 0);
        int j = 0;

        for (int i = 1; i < s.length(); i++) {
            while (j > 0 && s[i] != s[j]) {
                j = kmpTable[j - 1];
            }
            if (s[i] == s[j]) {
                j++;
            }
            kmpTable[i] = j;
        }

        return kmpTable;
    }
};
```

