# [Leetcode 72: Edit Distance](https://leetcode.com/problems/edit-distance/)

**Approaches:**
- [Approach 1: Recursive Solution (Basic)](#approach-1-recursive-solution-basic)
- [Approach 2: Recursive Solution with Memoization](#approach-2-recursive-solution-with-memoization)
- [Approach 3: Dynamic Programming (Optimal)](#approach-3-dynamic-programming-optimal)

## Approach 1: Recursive Solution (Basic)

### Intuition
The problem is looking for the minimum number of operations required to convert one string into another. A basic recursive solution would try to define the problem in terms of smaller sub-problems:
- If the last characters match, no operation is needed for them. Proceed to check the rest of the string.
- If they don’t match, consider three operations:
  1. Insert a character and check.
  2. Remove a character and check.
  3. Replace a character and check.

The minimum of these operations would be our answer for the sub-problem.

### Code
```java
public class EditDistance {

    // Recursive function to find the edit distance
    public int minDistance(String word1, String word2) {
        return calculateDistance(word1, word2, word1.length(), word2.length());
    }

    private int calculateDistance(String word1, String word2, int m, int n) {
        // Base case: If first string is empty, insert all characters of second string
        if (m == 0) return n;
        // Base case: If second string is empty, remove all characters of first string
        if (n == 0) return m;
        
        // If last characters are the same, ignore and recurse for remaining
        if (word1.charAt(m - 1) == word2.charAt(n - 1))
            return calculateDistance(word1, word2, m - 1, n - 1);

        // If last character is different, consider insert, remove and replace
        return 1 + Math.min(calculateDistance(word1, word2, m, n - 1), 
                            Math.min(calculateDistance(word1, word2, m - 1, n),
                                     calculateDistance(word1, word2, m - 1, n - 1)));
    }
}
```

### Time and Space Complexity
- **Time Complexity:** O(3^(m+n)), where m and n are the lengths of the two strings. This is because for each element, we make three recursive calls.
- **Space Complexity:** O(m + n), due to the recursion stack.

## Approach 2: Recursive Solution with Memoization

### Intuition
The basic recursive solution computes the same sub-problems multiple times. Using memoization, we can store the results of already computed sub-problems to avoid redundant calculations, thus optimizing the recursive approach.

### Code
```java
public class EditDistanceMemoization {

    // Memoization array
    private int[][] memo;

    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        memo = new int[m + 1][n + 1];
        
        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                memo[i][j] = -1; // init all values to -1
            }
        }
        
        return calculateDistance(word1, word2, m, n);
    }

    private int calculateDistance(String word1, String word2, int m, int n) {
        if (m == 0) return n;
        if (n == 0) return m;

        if (memo[m][n] != -1) return memo[m][n];

        if (word1.charAt(m - 1) == word2.charAt(n - 1)) {
            memo[m][n] = calculateDistance(word1, word2, m - 1, n - 1);
        } else {
            memo[m][n] = 1 + Math.min(calculateDistance(word1, word2, m, n - 1), 
                                      Math.min(calculateDistance(word1, word2, m - 1, n),
                                               calculateDistance(word1, word2, m - 1, n - 1)));
        }

        return memo[m][n];
    }
}
```

### Time and Space Complexity
- **Time Complexity:** O(m * n), since each subproblem is computed once.
- **Space Complexity:** O(m * n), for the memoization array and recursion stack.

## Approach 3: Dynamic Programming (Optimal)

### Intuition
The dynamic programming approach iteratively builds up solutions to smaller sub-problems to get the result for the original problem. This reduces the repeated computation caused by recursion.
- Create a 2D table `dp` where `dp[i][j]` represents the edit distance between the first `i` characters of `word1` and the first `j` characters of `word2`.
- The value of `dp[i][j]` is determined by:
  1. If the characters match, carry forward the diagonal value `dp[i-1][j-1]`.
  2. Else, take the minimum of inserting into `dp[i][j-1]`, removing from `dp[i-1][j]`, or replacing via `dp[i-1][j-1]` and add 1.

### Code
```java
public class EditDistanceDP {

    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m + 1][n + 1];

        // Initialize dp array for when one of the strings is empty
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }

        // Fill the dp table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = 1 + Math.min(dp[i][j - 1], 
                                            Math.min(dp[i - 1][j], dp[i - 1][j - 1]));
                }
            }
        }

        // The edit distance between full lengths of word1 and word2
        return dp[m][n];
    }
}
```

### Time and Space Complexity
- **Time Complexity:** O(m * n), because we fill out the entire dp table.
- **Space Complexity:** O(m * n), for the dp array.

