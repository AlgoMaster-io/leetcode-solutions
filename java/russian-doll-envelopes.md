# [Leetcode Problem 354: Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approaches
- [Approach 1: Dynamic Programming](#approach-1-dynamic-programming)
- [Approach 2: Sort and Longest Increasing Subsequence (LIS)](#approach-2-sort-and-longest-increasing-subsequence-lis)

## Approach 1: Dynamic Programming

### Intuition
The problem resembles the "Longest Increasing Subsequence" (LIS) problem. The main challenge is to properly define when one envelope can fit into another. An envelope `(w1, h1)` can fit into another envelope `(w2, h2)` if both `w1 < w2` and `h1 < h2`.

Dynamic programming can be used by sorting the envelopes based on width, and then computing the LIS based on the height of envelopes such that previous envelopes' width and height are both less than the current one.

### Steps
1. Sort the envelopes by width. If two envelopes have the same width, sort by height in descending order to avoid putting envelopes of the same width one inside the other.
2. Use dynamic programming to find the longest increasing subsequence of heights.

### Code
```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if (envelopes.length == 0) return 0;

        // Sort by width and then height in decreasing order for the same width
        Arrays.sort(envelopes, (a, b) -> {
            if (a[0] == b[0]) return b[1] - a[1];
            return a[0] - b[0];
        });
        
        int n = envelopes.length;
        int[] dp = new int[n];
        int maxEnvelopes = 0;

        // Initialize all dp values to 1
        Arrays.fill(dp, 1);

        // Dynamic Programming approach to find the LIS
        for (int i = 0; i < n; i++) {
            // Compare the current envelope with all previous envelopes
            for (int j = 0; j < i; j++) {
                if (envelopes[i][1] > envelopes[j][1]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            maxEnvelopes = Math.max(maxEnvelopes, dp[i]);
        }
        return maxEnvelopes;
    }
}
```

### Complexity
- Time Complexity: \( O(n^2) \) due to the double iteration to find the LIS.
- Space Complexity: \( O(n) \) for the `dp` array.

## Approach 2: Sort and Longest Increasing Subsequence (LIS)

### Intuition
Leverage the fact that when width is sorted, the problem simplifies to finding the LIS based on height. By sorting the second criteria (height) in descending order when widths are equal, we ensure no two envelopes of the same width contribute to the LIS.

Use a more optimal approach with a binary search to find the correct position of each height in the constructed LIS.

### Steps
1. Sort envelopes based on width in ascending order, and height in descending order when widths are equal.
2. Use the LIS approach with binary search to efficiently build the subsequence.

### Code
```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if (envelopes.length == 0) return 0;

        // Sort width ascending and height descending if widths are equal
        Arrays.sort(envelopes, (a, b) -> {
            if (a[0] == b[0]) return b[1] - a[1];
            return a[0] - b[0];
        });

        // Find the LIS on the heights
        List<Integer> dp = new ArrayList<>();

        for (int[] envelope : envelopes) {
            int height = envelope[1];
            int index = Collections.binarySearch(dp, height);

            if (index < 0) {
                index = -(index + 1);
            }
            if (index == dp.size()) {
                dp.add(height);
            } else {
                dp.set(index, height);
            }
        }
        
        return dp.size();
    }
}
```

### Complexity
- Time Complexity: \( O(n \log n) \) for sorting and binary search operations.
- Space Complexity: \( O(n) \) for storing the LIS.

