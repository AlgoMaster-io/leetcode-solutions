# [Leetcode 1014: Best Sightseeing Pair](https://leetcode.com/problems/best-sightseeing-pair)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized Single Pass](#approach-2-optimized-single-pass)

## Approach 1: Brute Force
The basic idea is to try all possible pairs `(i, j)` where `i < j` and calculate the score `A[i] + A[j] + i - j`, keeping track of the maximum score found.

### Intuition:
1. Loop through the array with two nested loops, selecting each pair `(i, j)`.
2. Compute the score for each pair.
3. Maintain a variable to store the maximum score observed.
4. Return the maximum score after checking all pairs.

### Time Complexity:
- O(n^2): Due to the nested loops over the array.

### Space Complexity:
- O(1): Only a few extra variables are used.

```cpp
int maxScoreSightseeingPair(vector<int>& A) {
    int maxScore = 0;
    int n = A.size();
    // Iterate over all pairs (i, j), i < j
    for (int i = 0; i < n - 1; ++i) {
        for (int j = i + 1; j < n; ++j) {
            // Calculate the score for the pair (i, j)
            int score = A[i] + A[j] + i - j;
            // Update maxScore if a better score is found
            maxScore = max(maxScore, score);
        }
    }
    return maxScore;
}
```

## Approach 2: Optimized Single Pass
Instead of evaluating every possible pair, we can keep track of the best scoring place up to the current index with a single pass through the list.

### Intuition:
1. Keep track of the best `A[i] + i` seen so far.
2. For each `j`, compute `A[j] - j` and add it to the best `A[i] + i` seen to get the potential score.
3. Keep updating the maximum potential score.
4. Update `A[i] + i` with the current element `A[j] + j` if it's better.

### Time Complexity:
- O(n): The array is traversed once.

### Space Complexity:
- O(1): Only a few extra variables are used.

```cpp
int maxScoreSightseeingPair(vector<int>& A) {
    int maxScore = 0;
    int bestA_i = A[0]; // Best A[i] + i; initialize with first element

    // Iterate over the array starting from the second element
    for (int j = 1; j < A.size(); ++j) {
        // Calculate the score for the pair (i, j) using the bestA_i
        maxScore = max(maxScore, bestA_i + A[j] - j);
        // Update bestA_i for the next iteration
        bestA_i = max(bestA_i, A[j] + j);
    }
    return maxScore;
}
```

In this approach, we systematically ensure that every possible `j` has the best possible previous `i` that could maximize the score with it due to our ongoing update of `bestA_i`. This exploits the problem constraints to condense what would be a quadratic problem into linear complexity by storing the valuable pre-computed state.

