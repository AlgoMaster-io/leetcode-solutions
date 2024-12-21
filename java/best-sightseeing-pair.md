# [1014. Best Sightseeing Pair](https://leetcode.com/problems/best-sightseeing-pair/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach with Precomputation](#optimized-approach-with-precomputation)
3. [Most Optimal Approach with One Pass](#most-optimal-approach-with-one-pass)

### Brute Force Approach

#### Intuition:
The brute force solution simply involves calculating the score for each possible pair of indices `(i, j)` where `i < j`. The score for a pair is defined as `A[i] + A[j] + i - j`. We iterate through all possible pairs and compute this score, keeping track of the maximum score encountered.

#### Code:
```java
public class BestSightseeingPair {
    public int maxScoreSightseeingPair(int[] A) {
        int maxScore = 0;
        // Loop through every pair of indices (i, j)
        for (int i = 0; i < A.length; i++) {
            for (int j = i + 1; j < A.length; j++) {
                // Compute score for this pair
                int score = A[i] + A[j] + i - j;
                // Update maxScore if this score is higher
                maxScore = Math.max(maxScore, score);
            }
        }
        return maxScore;
    }
}
```
#### Time Complexity:
- **O(n^2)**: We have a nested loop where each element is compared with every other element.
  
#### Space Complexity:
- **O(1)**: We are using a constant amount of extra space.

### Optimized Approach with Precomputation

#### Intuition:
This solution involves precomputing the maximum value of `A[i] + i` for each index, and then for each `j`, computing the maximum score using the precomputed values.

#### Code:
```java
public class BestSightseeingPair {
    public int maxScoreSightseeingPair(int[] A) {
        int[] maxLeft = new int[A.length];
        
        // Precompute maxLeft[i] which stores the maximum value of A[k] + k for all k <= i
        int maxVal = Integer.MIN_VALUE;
        for (int i = 0; i < A.length; i++) {
            maxVal = Math.max(maxVal, A[i] + i);
            maxLeft[i] = maxVal;
        }
        
        int maxScore = Integer.MIN_VALUE;
        // Compute max score using precomputed maxLeft values
        for (int j = 1; j < A.length; j++) {
            maxScore = Math.max(maxScore, maxLeft[j-1] + A[j] - j);
        }
        
        return maxScore;
    }
}
```
#### Time Complexity:
- **O(n)**: We make two passes over the array.

#### Space Complexity:
- **O(n)**: We use an additional array to store precomputed values.

### Most Optimal Approach with One Pass

#### Intuition:
The most efficient approach is to calculate `A[i] + i` on the go while iterating. We keep track of the maximum `A[i] + i` encountered so far and update the result with each `A[j] - j`. This allows us to solve the problem in a single pass with constant space.

#### Code:
```java
public class BestSightseeingPair {
    public int maxScoreSightseeingPair(int[] A) {
        int maxScore = Integer.MIN_VALUE;
        int maxVal = A[0] + 0; // Start with the first element
        
        // Loop through the array starting from the second element
        for (int j = 1; j < A.length; j++) {
            // Update the maximum score if the current score is higher
            maxScore = Math.max(maxScore, maxVal + A[j] - j);
            // Update maxVal to include the current j
            maxVal = Math.max(maxVal, A[j] + j);
        }
        
        return maxScore;
    }
}
```

#### Time Complexity:
- **O(n)**: We only need a single pass over the array.

#### Space Complexity:
- **O(1)**: We do not use any extra space beyond simple variables.

This final approach is both time optimal and space efficient, providing a clear improvement over the brute force solution by leveraging the problem's properties to reduce unnecessary calculations.

