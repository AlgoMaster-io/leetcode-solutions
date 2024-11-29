# 354. [Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approach 1: Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
using System;

public class Solution {
    public int MaxEnvelopes(int[][] envelopes) {
        if (envelopes.Length == 0) return 0;

        // Sort envelopes by width; if width is the same, sort by height descending
        Array.Sort(envelopes, (a, b) => {
            if (a[0] == b[0]) {
                return b[1] - a[1];
            } else {
                return a[0] - b[0];
            }
        });

        int n = envelopes.Length;
        int[] dp = new int[n];
        Array.Fill(dp, 1);

        int maxEnvelopes = 0;

        // Dynamic programming to find the maximum number of envelopes
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                // Check if j can fit into i
                if (envelopes[j][1] < envelopes[i][1] && envelopes[j][0] < envelopes[i][0]) {
                    dp[i] = Math.Max(dp[i], dp[j] + 1);
                }
            }
            maxEnvelopes = Math.Max(maxEnvelopes, dp[i]);
        }

        return maxEnvelopes;
    }
}
```

## Approach 2: Patience Sorting and Longest Increasing Subsequence (LIS)

### Solution
csharp
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int MaxEnvelopes(int[][] envelopes) {
        if (envelopes.Length == 0) return 0;

        // Sort envelopes by width; if width is the same, sort by height descending
        Array.Sort(envelopes, (a, b) => {
            if (a[0] == b[0]) {
                return b[1] - a[1];
            } else {
                return a[0] - b[0];
            }
        });

        // List to hold our increasing sequence of heights
        List<int> lis = new List<int>();

        foreach (var envelope in envelopes) {
            int height = envelope[1];

            // Perform binary search to find the correct position to replace or add
            int pos = lis.BinarySearch(height);

            // If position is negative, BinarySearch returns a negative number indicating the bitwise complement of the index of the next element
            if (pos < 0) pos = ~pos;

            // Add or replace the value at the identified position
            if (pos >= lis.Count) {
                lis.Add(height);
            } else {
                lis[pos] = height;
            }
        }

        return lis.Count;
    }
}
```

