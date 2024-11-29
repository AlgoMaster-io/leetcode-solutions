# 139. [Word Break](https://leetcode.com/problems/word-break/)

## Approach 1: Recursion with Memoization

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <iostream>
#include <vector>
#include <unordered_map>

class Solution {
public:
    bool wordBreak(std::string s, std::vector<std::string>& wordDict) {
        std::unordered_map<std::string, bool> memo;
        return wordBreakHelper(s, wordDict, memo);
    }

private:
    bool wordBreakHelper(const std::string& s, const std::vector<std::string>& wordDict, std::unordered_map<std::string, bool>& memo) {
        if (s.empty()) {
            return true;
        }

        if (memo.find(s) != memo.end()) {
            return memo[s];
        }

        // Check if any prefix of 's' is a word in wordDict
        for (const std::string& word : wordDict) {
            if (s.substr(0, word.length()) == word && wordBreakHelper(s.substr(word.length()), wordDict, memo)) {
                memo[s] = true;
                return true;
            }
        }

        memo[s] = false;
        return false;
    }
};
```

## Approach 2: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <iostream>
#include <vector>
#include <unordered_set>

class Solution {
public:
    bool wordBreak(std::string s, const std::vector<std::string>& wordDict) {
        std::unordered_set<std::string> wordSet(wordDict.begin(), wordDict.end());
        std::vector<bool> dp(s.length() + 1, false);
        dp[0] = true; // Base case, empty string

        // Traverse the string to build DP table
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                // If substring(0, j) can be segmented and substring(j, i) is in wordSet
                if (dp[j] && wordSet.count(s.substr(j, i - j))) {
                    dp[i] = true;
                    break; // Move on to next i since we found a valid segmentation
                }
            }
        }

        return dp[s.length()]; // Result is whether the whole string can be segmented
    }
};
```

Let me know if you need any further explanation!

