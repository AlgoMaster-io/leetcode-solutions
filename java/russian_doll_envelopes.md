# 354. [Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approach 1: Dynamic Programming

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n)
import java.util.Arrays;

public class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if (envelopes.length == 0) return 0;

        // Sort envelopes by width; if width is the same, sort by height descending
        Arrays.sort(envelopes, (a, b) -> {
            if (a[0] == b[0]) {
                return b[1] - a[1];
            } else {
                return a[0] - b[0];
            }
        });

        int n = envelopes.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);

        int maxEnvelopes = 0;

        // Dynamic programming to find the maximum number of envelopes
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                // Check if j can fit into i
                if (envelopes[j][1] < envelopes[i][1] && envelopes[j][0] < envelopes[i][0]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            maxEnvelopes = Math.max(maxEnvelopes, dp[i]);
        }

        return maxEnvelopes;
    }
}
```

## Approach 2: Patience Sorting and Longest Increasing Subsequence (LIS)

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import java.util.Arrays;
import java.util.ArrayList;

public class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if (envelopes.length == 0) return 0;

        // Sort envelopes by width; if width is the same, sort by height descending
        Arrays.sort(envelopes, (a, b) -> {
            if (a[0] == b[0]) {
                return b[1] - a[1];
            } else {
                return a[0] - b[0];
            }
        });

        // Array to hold our increasing sequence of heights
        ArrayList<Integer> lis = new ArrayList<>();

        for (int[] envelope : envelopes) {
            int height = envelope[1];

            // Perform binary search to find the correct position to replace or add
            int pos = binarySearch(lis, height);

            // If position is equal to the size of the list, we don't find a suitable place
            if (pos < 0) pos = -(pos + 1);

            // Add or replace the value at the identified position
            if (pos >= lis.size()) {
                lis.add(height);
            } else {
                lis.set(pos, height);
            }
        }

        return lis.size();
    }

    private int binarySearch(ArrayList<Integer> lis, int key) {
        int low = 0, high = lis.size() - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (lis.get(mid) < key) low = mid + 1;
            else high = mid - 1;
        }
        return low;
    }
}
```

