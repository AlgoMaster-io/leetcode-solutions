# [Best Sightseeing Pair](https://leetcode.com/problems/best-sightseeing-pair/)

### Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach](#optimized-approach)

## Brute Force Approach

### Intuition
The brute force approach involves checking every possible pair of places to determine the best sightseeing experience, defined by `A[i] + A[j] + i - j`. For each pair `(i, j)` where `j > i`, we calculate their value and keep track of the maximum value encountered.

### Code
```javascript
function maxScoreSightseeingPair(A) {
    let maxScore = 0;
    let n = A.length;

    // Attempt every pair of points
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            // Calculate the sightseeing score for the pair (i, j)
            let score = A[i] + A[j] + i - j;
            // Update maxScore if we found a better score
            maxScore = Math.max(maxScore, score);
        }
    }
    
    return maxScore;
}
```

### Time and Space Complexity
- **Time Complexity**: `O(n^2)` where `n` is the number of elements in the array. This is because we evaluate each pair `(i, j)` with `j > i`.
- **Space Complexity**: `O(1)` since no extra space is used apart from simple variables.

## Optimized Approach

### Intuition
To optimize the process, we can break down the term `A[i] + i + A[j] - j`. Notice that for any `j`, the score `A[i] + i` must be evaluated against `A[j] - j`. Thus, our goal is to keep track of the best value of `A[i] + i` encountered so far for any index `j`. By storing this best result, we calculate the score for each `j` in constant time, and we also update the best `A[i] + i` for subsequent indices.

### Code
```javascript
function maxScoreSightseeingPair(A) {
    let maxScore = 0;
    let maxI = A[0]; // maxA[i] + i for the first element

    for (let j = 1; j < A.length; j++) {
        // Max score with j being the right point, considering maxI as the best left point
        maxScore = Math.max(maxScore, maxI + A[j] - j);

        // Update maxI for the next iterations
        maxI = Math.max(maxI, A[j] + j);
    }
    
    return maxScore;
}
```

### Time and Space Complexity
- **Time Complexity**: `O(n)` since we only iterate through the array once.
- **Space Complexity**: `O(1)` as we store only a fixed number of variables regardless of the input size.

By efficiently keeping track of the required values during a single traversal of the array, we reduce the time complexity significantly when compared to the brute force approach.

