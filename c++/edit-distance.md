# LeetCode Problem 72: Edit Distance

[Edit Distance - LeetCode](https://leetcode.com/problems/edit-distance/)

## Approaches:
1. [Recursive Solution](#recursive-solution)
2. [Dynamic Programming Solution](#dynamic-programming-solution)
3. [Optimized Dynamic Programming Solution](#optimized-dynamic-programming-solution)

### Recursive Solution

The recursive approach is an intuitive way to solve the edit distance problem. However, it is not efficient for large strings due to its exponential time complexity. The idea is to recursively try all operations -- insert, delete, and replace -- and select the operation that incurs the minimum cost.

```cpp
// Recursive solution for the edit distance problem
class Solution {
public:
    // Helper function to compute the edit distance using recursion
    int minDistanceHelper(string &word1, string &word2, int m, int n) {
        // Base cases:
        // If one of the strings is empty, the edit distance is the length of the other string
        if (m == 0) return n;
        if (n == 0) return m;

        // If last characters of both substrings are same, continue with the remaining strings
        if (word1[m - 1] == word2[n - 1]) {
            return minDistanceHelper(word1, word2, m - 1, n - 1);
        }

        // If the last characters are different, consider all three operations
        // and choose the one with the minimum cost:
        int insertOp = minDistanceHelper(word1, word2, m, n - 1);
        int deleteOp = minDistanceHelper(word1, word2, m - 1, n);
        int replaceOp = minDistanceHelper(word1, word2, m - 1, n - 1);

        // Insert: Add a character to make the last characters equal
        // Delete: Remove a character and process the rest
        // Replace: Replace the current character and check rest
        return 1 + min({insertOp, deleteOp, replaceOp});
    }

    int minDistance(string word1, string word2) {
        return minDistanceHelper(word1, word2, word1.size(), word2.size());
    }
};
```

- **Time Complexity:** O(3^(m+n)) where m and n are the lengths of the two strings. This is because in the worst case, each operation (insert, delete, replace) is tried for each character.
- **Space Complexity:** O(m + n) due to the recursive stack.

### Dynamic Programming Solution

The DP approach stores the result of overlapping subproblems to avoid redundant calculations, significantly improving efficiency. We use a 2D array where `dp[i][j]` represents the edit distance between the first `i` characters of `word1` and the first `j` characters of `word2`.

```cpp
// DP solution for the edit distance problem
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));

        // Initialize the first row and column as base cases
        for (int i = 0; i <= m; ++i) dp[i][0] = i;
        for (int j = 0; j <= n; ++j) dp[0][j] = j;

        // Fill the DP table
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (word1[i - 1] == word2[j - 1]) {
                    // If the characters match, carry forward the edit distance from the previous step
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // Consider insert, delete, and replace operations
                    dp[i][j] = 1 + min({dp[i - 1][j],    // delete
                                        dp[i][j - 1],    // insert
                                        dp[i - 1][j - 1] // replace
                                       });
                }
            }
        }

        return dp[m][n];
    }
};
```

- **Time Complexity:** O(m*n) for filling up the DP table.
- **Space Complexity:** O(m*n) for storing the DP matrix.

### Optimized Dynamic Programming Solution

We can optimize the space complexity by only maintaining the last two rows of the DP table, as each computation only relies on the current and previous rows.

```cpp
// Optimized Space DP solution for the edit distance problem
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<int> prev(n + 1, 0), curr(n + 1, 0);

        // Initialize base case for the first row
        for (int j = 0; j <= n; ++j) prev[j] = j;

        for (int i = 1; i <= m; ++i) {
            curr[0] = i; // Base case for column
            for (int j = 1; j <= n; ++j) {
                if (word1[i - 1] == word2[j - 1]) {
                    curr[j] = prev[j - 1];
                } else {
                    curr[j] = 1 + min({prev[j],    // delete
                                       curr[j - 1], // insert
                                       prev[j - 1]  // replace
                                      });
                }
            }
            prev = curr; // Shift rows
        }

        return prev[n];
    }
};
```

- **Time Complexity:** O(m*n) for filling up the DP table.
- **Space Complexity:** O(n) to store the current and previous row only.

