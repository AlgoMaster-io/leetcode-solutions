# [Leetcode 354: Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approaches

1. [Dynamic Programming Approach](#dynamic-programming-approach)
2. [Patience Sort (LIS Variation) Approach](#patience-sort-lis-variation-approach)

---

## Dynamic Programming Approach

### Intuition

The Russian Doll Envelopes problem can be reduced to finding the longest increasing subsequence (LIS) when envelopes are ordered by width. However, two envelopes of the same width cannot nest inside each other. Thus, for envelopes of equal width, we should order them by height in descending order. This ensures we avoid invalid configurations where two identical-width envelopes are considered in the LIS.

Here's how we can approach this problem using dynamic programming:
1. Sort the envelopes by width in ascending order. If the widths are the same, sort by height in descending order.
2. Apply a dynamic programming approach to find the LIS based on the heights.

### Solution

```typescript
function maxEnvelopes(envelopes: number[][]): number {
    if (!envelopes || envelopes.length === 0) return 0;

    // Sort the envelopes. If two envelopes have the same width, sort by height descending.
    envelopes.sort((a, b) => {
        if (a[0] === b[0]) {
            return b[1] - a[1];
        }
        return a[0] - b[0];
    });

    let n = envelopes.length;
    let dp = new Array(n).fill(1);

    let maxCount = 1;
    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            // If the current envelope can fit into the j-th envelope
            if (envelopes[i][1] > envelopes[j][1]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        maxCount = Math.max(maxCount, dp[i]);
    }
    
    return maxCount;
}
```

### Time and Space Complexity

- **Time Complexity:** \(O(n^2)\), where \(n\) is the number of envelopes. This is due to the double loop used in the dynamic programming approach.
- **Space Complexity:** \(O(n)\), for the dynamic programming table used to store the LIS counts.

---

## Patience Sort (LIS Variation) Approach

### Intuition

By recognizing the problem as an LIS problem on the heights after sorting the envelopes by width, we can improve the efficiency using the Patience Sorting technique, which can find the LIS in \(O(n \log n)\) time.

1. Sort the envelopes: Order by width ascend and by height descend for equal widths.
2. Use a binary search to find the position to replace or extend the current subsequence of heights.

### Solution

```typescript
function maxEnvelopesAdvanced(envelopes: number[][]): number {
    if (!envelopes || envelopes.length === 0) return 0;

    envelopes.sort((a, b) => {
        if (a[0] === b[0]) {
            return b[1] - a[1];
        }
        return a[0] - b[0];
    });

    let heights: number[] = [];
    for (let envelope of envelopes) {
        let height = envelope[1];

        // Binary search to find the correct position to place the current envelope's height
        let left = 0, right = heights.length;
        while (left < right) {
            let mid = Math.floor((left + right) / 2);
            if (heights[mid] < height) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        // If left is beyond the current heights length, we extend the list
        if (left === heights.length) {
            heights.push(height);
        } else {
            heights[left] = height;
        }
    }

    return heights.length;
}
```

### Time and Space Complexity

- **Time Complexity:** \(O(n \log n)\), where \(n\) is the number of envelopes. Sorting takes \(O(n \log n)\) and finding LIS using binary search takes \(O(n \log n)\).
- **Space Complexity:** \(O(n)\), utilized by the list maintaining the current LIS.

