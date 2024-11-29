# 354. [Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approach 1: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
function maxEnvelopes(envelopes) {
    if (envelopes.length === 0) return 0;

    // Sort envelopes by width; if width is the same, sort by height descending
    envelopes.sort((a, b) => {
        if (a[0] === b[0]) {
            return b[1] - a[1];
        } else {
            return a[0] - b[0];
        }
    });

    const n = envelopes.length;
    const dp = Array(n).fill(1);

    let maxEnvelopes = 0;

    // Dynamic programming to find the maximum number of envelopes
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < i; j++) {
            // Check if j can fit into i
            if (envelopes[j][1] < envelopes[i][1] && envelopes[j][0] < envelopes[i][0]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        maxEnvelopes = Math.max(maxEnvelopes, dp[i]);
    }

    return maxEnvelopes;
}
```

## Approach 2: Patience Sorting and Longest Increasing Subsequence (LIS)

### Solution
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
function maxEnvelopes(envelopes) {
    if (envelopes.length === 0) return 0;

    // Sort envelopes by width; if width is the same, sort by height descending
    envelopes.sort((a, b) => {
        if (a[0] === b[0]) {
            return b[1] - a[1];
        } else {
            return a[0] - b[0];
        }
    });

    // Array to hold our increasing sequence of heights
    const lis = [];

    for (const envelope of envelopes) {
        const height = envelope[1];

        // Perform binary search to find the correct position to replace or add
        let pos = binarySearch(lis, height);

        // If position is equal to the size of the list, we don't find a suitable place
        if (pos < 0) pos = -(pos + 1);

        // Add or replace the value at the identified position
        if (pos >= lis.length) {
            lis.push(height);
        } else {
            lis[pos] = height;
        }
    }

    return lis.length;
}

function binarySearch(lis, key) {
    let low = 0, high = lis.length - 1;
    while (low <= high) {
        const mid = low + Math.floor((high - low) / 2);
        if (lis[mid] < key) low = mid + 1;
        else high = mid - 1;
    }
    return low;
}
```

