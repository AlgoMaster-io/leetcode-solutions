# [Leetcode 1014: Best Sightseeing Pair](https://leetcode.com/problems/best-sightseeing-pair/)

### Navigation
- [Approach 1: Brute Force Approach](#approach-1-brute-force-approach)
- [Approach 2: Optimized Dynamic Programming](#approach-2-optimized-dynamic-programming)

## Approach 1: Brute Force Approach

### Intuition
The brute force approach involves examining all pairs of indices `(i, j)` (where `i < j`) and checking the value of `A[i] + A[j] + i - j`. By iterating through all possible pairs, we can find the maximum value of the function. This approach is straightforward but inefficient due to its time complexity.

### Implementation

```typescript
function maxScoreSightseeingPair(A: number[]): number {
    let maxScore = 0;

    // Loop through each pair (i, j)
    for (let i = 0; i < A.length; i++) {
        for (let j = i + 1; j < A.length; j++) {
            // Calculate the score for the pair (i, j)
            const score = A[i] + A[j] + i - j;
            // Update the maximum score if necessary
            maxScore = Math.max(maxScore, score);
        }
    }

    return maxScore;
}
```

### Complexity Analysis
- **Time Complexity**: O(n^2), where n is the length of the input array. This is because for each element, we are iterating over all subsequent elements.
- **Space Complexity**: O(1), as we are using a constant amount of additional space.

## Approach 2: Optimized Dynamic Programming

### Intuition
The optimal approach uses dynamic programming by maintaining a running maximum for the term `A[i] + i`, which allows us to calculate the function more efficiently. As we iterate through the array, we update the maximum score using the previously calculated maximum and the current element adjusted with `- j`.

### Implementation

```typescript
function maxScoreSightseeingPair(A: number[]): number {
    let maxScore = 0;
    let maxI = A[0] + 0; // Initial max based on the first element and index

    // Iterate through array starting from the second element
    for (let j = 1; j < A.length; j++) {
        // Calculate the score for i, j pair
        maxScore = Math.max(maxScore, maxI + A[j] - j);
        // Update maxI for next iteration
        maxI = Math.max(maxI, A[j] + j);
    }

    return maxScore;
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the input array. We only pass through the array once.
- **Space Complexity**: O(1), as no additional data structures are used apart from a few variables.

This optimal approach significantly improves the efficiency of solving the problem by reducing the time complexity to linear time while maintaining constant space usage.

