# LeetCode Problem: [132. Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approaches

- [Approach 1: Dynamic Programming with Recursive Helper Function](#approach-1-dynamic-programming-with-recursive-helper-function)
- [Approach 2: Optimized Dynamic Programming with Iterative Approach](#approach-2-optimized-dynamic-programming-with-iterative-approach)
- [Approach 3: Dynamic Programming with Improved Space Complexity](#approach-3-dynamic-programming-with-improved-space-complexity)

---

## Approach 1: Dynamic Programming with Recursive Helper Function

### Intuition
The idea is to use dynamic programming to find the minimum cuts required to partition the string such that every substring is a palindrome. We'll define a helper function to recursively find this minimum cut count and store intermediate solutions to avoid redundant computations.

### Steps:
1. Use a helper function `isPalindrome` to check if a substring is a palindrome.
2. Define a `dp` vector where `dp[i]` represents the minimum cuts needed for the substring `s[i...n-1]`.
3. Use a recursive function to fill this `dp` table, exploring all possible palindrome substrings starting from `i`.
4. If a substring `s[i...j]` is a palindrome, consider `1 + dp[j+1]` as the possible minimum cuts for `s[i...n-1]`.
5. Return `dp[0]` which gives the result for string `s`.

### Code
```cpp
class Solution {
public:
    int minCut(string s) {
        int n = s.size();
        // Initialize dp array with maximum cuts
        vector<int> dp(n, INT_MAX);

        // Helper function to verify if a substring is a palindrome
        auto isPalindrome = [&](int start, int end) -> bool {
            while (start < end) {
                if (s[start++] != s[end--]) return false;
            }
            return true;
        };

        // Recursive function to compute minimum cuts
        function<int(int)> minCuts = [&](int start) -> int {
            if (start == n) return -1; // No cuts needed beyond the string
            if (dp[start] != INT_MAX) return dp[start];

            for (int end = start; end < n; ++end) {
                if (isPalindrome(start, end)) {
                    dp[start] = min(dp[start], 1 + minCuts(end + 1));
                }
            }
            return dp[start];
        };

        return minCuts(0);
    }
};
```

### Complexity
- **Time Complexity:** \(O(n^3)\) due to palindrome checks and recursion.
- **Space Complexity:** \(O(n)\) for the `dp` array.

---

## Approach 2: Optimized Dynamic Programming with Iterative Approach

### Intuition
Previous approach can be optimized to avoid excessive use of recursive calls. Instead, solve the problem iteratively while still utilizing dynamic programming.

### Steps:
1. Create a 2D matrix `dp` to precompute palindrome substrings. 
2. Use dynamic programming to solve the palindrome partitioning by filling the `minCut` array iteratively.
3. Determine `minCut[i]` from previously computed palindrome results.

### Code
```cpp
class Solution {
public:
    int minCut(string s) {
        int n = s.size();
        vector<vector<bool>> dp(n, vector<bool>(n, false));
        vector<int> minCut(n, 0);

        for (int i = n - 1; i >= 0; --i) {
            minCut[i] = n - i - 1; // Maximum initial cuts possible
            for (int j = i; j < n; ++j) {
                if (s[i] == s[j] && (j - i < 2 || dp[i + 1][j - 1])) {
                    dp[i][j] = true;
                    if (j == n - 1) {
                        minCut[i] = 0;  // Whole string is a palindrome
                    } else {
                        minCut[i] = min(minCut[i], 1 + minCut[j + 1]);
                    }
                }
            }
        }
        return minCut[0];
    }
};
```

### Complexity
- **Time Complexity:** \(O(n^2)\) due to 2 nested loops and dynamic programming.
- **Space Complexity:** \(O(n^2)\) for the `dp` table.

---

## Approach 3: Dynamic Programming with Improved Space Complexity

### Intuition
Further optimize the space complexity by using a single array to track the palindrome status and the minimum cuts.

### Steps:
1. Use a boolean vector to represent if a substring is a palindrome.
2. Maintain a vector for minimum cuts with more optimal space usage.
3. Update palindrome status and minimum cuts in a single pass.

### Code
```cpp
class Solution {
public:
    int minCut(string s) {
        int n = s.size();
        vector<int> minCut(n, 0);

        for (int i = 0; i < n; ++i) {
            minCut[i] = i;  // Maximum cuts is cutting between every character
        }

        for (int mid = 0; mid < n; ++mid) {
            // Check for odd-length palindromes
            for (int start = mid, end = mid; start >= 0 && end < n && s[start] == s[end]; --start, ++end) {
                minCut[end] = start == 0 ? 0 : min(minCut[end], minCut[start - 1] + 1);
            }
            // Check for even-length palindromes
            for (int start = mid, end = mid + 1; start >= 0 && end < n && s[start] == s[end]; --start, ++end) {
                minCut[end] = start == 0 ? 0 : min(minCut[end], minCut[start - 1] + 1);
            }
        }
        
        return minCut[n - 1];
    }
};
```

### Complexity
- **Time Complexity:** \(O(n^2)\) due to nested loops to check for palindromes.
- **Space Complexity:** \(O(n)\) using only a single array for minimum cuts and palindrome checks.

This approach is both time-efficient and space-efficient, making it an optimal solution for the problem.

