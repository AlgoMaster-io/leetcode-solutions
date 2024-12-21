# [Leetcode 1014: Best Sightseeing Pair](https://leetcode.com/problems/best-sightseeing-pair/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach](#optimized-approach)

---

### Brute Force Approach

#### Intuition

The brute force solution involves checking every possible pair of sightseeing spots `(i, j)` where `i < j` and calculating their combined value `A[i] + A[j] + i - j`. The idea is to iterate over each possible pair and determine which combination yields the maximum value.

#### Implementation

```csharp
public class Solution {
    public int MaxScoreSightseeingPair(int[] A) {
        int maxScore = 0;
        
        // Iterate over each possible pair of spots
        for (int i = 0; i < A.Length; i++) {
            for (int j = i + 1; j < A.Length; j++) {
                // Calculate the score for the pair (i, j)
                int score = A[i] + A[j] + i - j;
                // Update maxScore if the current pair's score is higher
                maxScore = Math.Max(maxScore, score);
            }
        }
        
        return maxScore;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(n^2)\) because we use a nested loop to check each pair.
- **Space Complexity:** \(O(1)\) as we are using a constant amount of extra space.

---

### Optimized Approach

#### Intuition

We can optimize the brute force solution by considering the formula `A[i] + A[j] + i - j` as `A[i] + i + A[j] - j`. The expression `A[i] + i` can be precomputed and stored as a variable, reducing the need for redundant calculations. We keep track of the maximum value seen for `A[i] + i` as we traverse the array. For each `A[j] - j`, we add the maximum `A[i] + i` seen so far to determine if this combination provides a higher score.

#### Implementation

```csharp
public class Solution {
    public int MaxScoreSightseeingPair(int[] A) {
        int n = A.Length;
        int maxScore = 0;
        int maxAiPlusI = A[0] + 0;
        
        for (int j = 1; j < n; j++) {
            // Calculate potential max score for the pair (maxAiPlusI, j)
            maxScore = Math.Max(maxScore, maxAiPlusI + A[j] - j);
            // Update maxAiPlusI for the next iteration
            maxAiPlusI = Math.Max(maxAiPlusI, A[j] + j);
        }
        
        return maxScore;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(n)\) as we only make a single pass through the array.
- **Space Complexity:** \(O(1)\) since the solution uses a constant amount of extra space.

---

Both solutions yield the correct results, but the optimized approach is significantly faster for large input sizes due to its linear time complexity. The key was breaking down the expression and recalculating only necessary components efficiently as we iterate through the array.

