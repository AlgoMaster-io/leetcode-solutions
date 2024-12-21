# [Leetcode 131: Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

## Approaches
- [Backtracking Solution](#backtracking-solution)
- [Optimized Backtracking with Memoization](#optimized-backtracking-with-memoization)

### Backtracking Solution

**Intuition**:  
The problem asks us to split the input string into all possible palindrome partitions. A natural approach for generating combinations like this is backtracking. We can use a helper function to attempt to split the string at every position, check if the obtained substring is a palindrome, and then recursively process the remainder of the string.

**Approach**:
1. Use a recursive function to try splitting the string at each position.
2. Check whether the current substring is a palindrome.
3. If it is a palindrome, recursively call the function for the remaining part of the string.
4. When we reach the end of the string, we add the current combination of palindromes to the result list.

```cpp
class Solution {
public:
    // Utility function to check if a string is a palindrome
    bool isPalindrome(const std::string &s, int start, int end) {
        while (start < end) {
            if (s[start] != s[end]) return false;
            start++;
            end--;
        }
        return true;
    }
    
    // Backtracking utility function
    void backtrack(int start, std::string &s, std::vector<std::string> &current, std::vector<std::vector<std::string>> &result) {
        // If we have reached the end of the string, add the current partition to the result
        if (start == s.size()) {
            result.push_back(current);
            return;
        }
        
        // Try to partition the string at different points
        for (int end = start; end < s.size(); ++end) {
            if (isPalindrome(s, start, end)) {
                // Choose
                current.push_back(s.substr(start, end - start + 1));
                // Explore
                backtrack(end + 1, s, current, result);
                // Unchoose
                current.pop_back();
            }
        }
    }
    
    // Main function to return all possible palindrome partitioning
    std::vector<std::vector<std::string>> partition(std::string s) {
        std::vector<std::vector<std::string>> result;
        std::vector<std::string> current;
        backtrack(0, s, current, result);
        return result;
    }
};
```

**Time Complexity**: O(N * 2^N), where N is the length of the string. Every substring can either be a part of a palindrome partition or not, leading to 2^N combinations, and for each combination, we potentially consider every substring.

**Space Complexity**: O(N), for the recursion stack and storing current partitions.

### Optimized Backtracking with Memoization

**Intuition**:  
The naive backtracking solution can be further optimized using memoization. Particularly, we can cache whether a substring is a palindrome to avoid recomputing it multiple times. This helps to significantly reduce repeated computations.

**Approach**:
1. Precompute and store in a 2-dimensional boolean array, `dp`, whether a substring from i to j is a palindrome.
2. Use the `dp` table to avoid recomputation of palindrome checks during backtracking.

```cpp
class Solution {
public:
    // Function to compute and memoize palindrome checks
    void precomputePalindromes(std::string &s, std::vector<std::vector<bool>> &dp) {
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            dp[i][i] = true; // single character is always a palindrome
        }
        for (int i = 0; i+1 < n; ++i) {
            dp[i][i+1] = (s[i] == s[i+1]); // check for two-character palindromes
        }
        for (int length = 3; length <= n; ++length) {
            for (int i = 0; i <= n-length; ++i) {
                int j = i + length - 1;
                dp[i][j] = (s[i] == s[j] && dp[i+1][j-1]);
            }
        }
    }

    void backtrack(int start, std::string &s, std::vector<std::string> &current, std::vector<std::vector<std::string>> &result, std::vector<std::vector<bool>> &dp) {
        if (start == s.size()) {
            result.push_back(current);
            return;
        }
        
        for (int end = start; end < s.size(); ++end) {
            // Use precomputed palindrome result
            if (dp[start][end]) {
                current.push_back(s.substr(start, end - start + 1));
                backtrack(end + 1, s, current, result, dp);
                current.pop_back();
            }
        }
    }

    std::vector<std::vector<std::string>> partition(std::string s) {
        int n = s.size();
        std::vector<std::vector<bool>> dp(n, std::vector<bool>(n, false));
        precomputePalindromes(s, dp);
        
        std::vector<std::vector<std::string>> result;
        std::vector<std::string> current;
        backtrack(0, s, current, result, dp);
        return result;
    }
};
```

**Time Complexity**: O(N^2 * 2^N). The precomputation of palindromes takes O(N^2), and the backtracking process has complexity similar to the naive approach.

**Space Complexity**: O(N^2) for the `dp` table to store palindrome checks, and O(N) for the recursion stack and current partition storage.

