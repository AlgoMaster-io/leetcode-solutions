# [Leetcode 139: Word Break](https://leetcode.com/problems/word-break/)

## Approaches
1. [Brute Force Recursion](#approach-1-brute-force-recursion)
2. [Recursion with Memoization](#approach-2-recursion-with-memoization)
3. [Dynamic Programming](#approach-3-dynamic-programming)

---

## Approach 1: Brute Force Recursion

### Intuition
The simplest way to solve this problem is to use recursion. For each possible prefix of the string `s`, check if it's in the word dictionary, and then recursively check if the remaining suffix can be segmented. This approach tries all possible segmentations of the string `s`.

### Code
```cpp
#include <iostream>
#include <vector>
#include <string>

bool wordBreakRecursion(const std::string& s, const std::vector<std::string>& wordDict, int start) {
    if (start == s.size()) {
        return true;
    }

    for (const std::string& word : wordDict) {
        int len = word.length();
        int end = start + len;
        
        // Check if the current word in wordDict can match a portion of the string s
        if (end <= s.size() && s.substr(start, len) == word) {
            // Recursively check for the remaining substring
            if (wordBreakRecursion(s, wordDict, end)) {
                return true;
            }
        }
    }
    
    return false;
}

bool wordBreak(std::string s, std::vector<std::string>& wordDict) {
    return wordBreakRecursion(s, wordDict, 0);
}
```

### Time Complexity
- O(n * 2^n), where n is the length of the string `s`. Each substring can either be segmented or not.

### Space Complexity
- O(n), due to the recursion stack depth.

---

## Approach 2: Recursion with Memoization

### Intuition
The brute force approach repeatedly checks the same substrings, leading to redundant calculations. By using memoization, we can store results for substrings that have already been computed, significantly reducing the number of computations.

### Code
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <unordered_map>

bool wordBreakMemoization(const std::string& s, const std::vector<std::string>& wordDict, int start, std::unordered_map<int, bool>& memo) {
    if (start == s.size()) {
        return true;
    }

    if (memo.find(start) != memo.end()) {
        return memo[start];
    }

    for (const std::string& word : wordDict) {
        int len = word.length();
        int end = start + len;

        if (end <= s.size() && s.substr(start, len) == word) {
            if (wordBreakMemoization(s, wordDict, end, memo)) {
                return memo[start] = true;
            }
        }
    }

    return memo[start] = false;
}

bool wordBreak(std::string s, std::vector<std::string>& wordDict) {
    std::unordered_map<int, bool> memo;
    return wordBreakMemoization(s, wordDict, 0, memo);
}
```

### Time Complexity
- O(n^2 * m), where n is the length of the string `s` and m is the size of the word dictionary. We check segments of `s` against words in `wordDict`.

### Space Complexity
- O(n), for the memoization hashmap and recursion stack.

---

## Approach 3: Dynamic Programming

### Intuition
We can use dynamic programming to build an array `dp` where `dp[i]` indicates whether the substring `s[0:i]` can be segmented into words from `wordDict`. We iterate over each possible end of a word, checking if there exists a valid start for this word if the substring ends at `i`.

### Code
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <unordered_set>

bool wordBreak(std::string s, std::vector<std::string>& wordDict) {
    std::unordered_set<std::string> wordSet(wordDict.begin(), wordDict.end());
    int n = s.size();
    std::vector<bool> dp(n+1, false);
    dp[0] = true; // Base case: empty string can always be segmented

    for (int i = 1; i <= n; ++i) {
        for (int j = 0; j < i; ++j) {
            // If the substring s[j:i] is in the wordDict and we can partition s[0:j],
            // then we can partition s[0:i]
            if (dp[j] && wordSet.find(s.substr(j, i - j)) != wordSet.end()) {
                dp[i] = true;
                break;
            }
        }
    }

    return dp[n];
}
```

### Time Complexity
- O(n^2), where n is the length of the string `s`. There is a double loop iterating through the string.

### Space Complexity
- O(n), for the dp array storing results for each index of `s`.

---

These three approaches give you a range of solutions from brute force to dynamic programming, catering to different complexities and efficiencies in handling the problem of word segmentation.

