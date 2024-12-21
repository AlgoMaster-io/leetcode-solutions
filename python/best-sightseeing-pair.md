### [Leetcode 1014: Best Sightseeing Pair](https://leetcode.com/problems/best-sightseeing-pair/)

#### Table of Contents
1. [Approach 1: Brute Force](#approach-1-brute-force)
2. [Approach 2: Optimized One Pass Calculation](#approach-2-optimized-one-pass-calculation)

---

### Approach 1: Brute Force

#### Intuition:
The brute force approach considers each pair of points and calculates the potential sightseeing value. By iterating with two nested loops, we calculate the value of each pair and keep track of the maximum found.

#### Approach:
1. Use two nested loops to consider every pair.
2. For a pair `(i, j)` where `i < j`, calculate the sightseeing value using the formula `A[i] + A[j] + i - j`.
3. Track the maximum value found during these calculations.

#### Code:
```python
def maxScoreSightseeingPair(A):
    n = len(A)
    max_score = 0
    # Iterate over each pair of points
    for i in range(n):
        for j in range(i + 1, n):
            # Calculate the sightseeing value for the pair (i, j)
            current_score = A[i] + A[j] + i - j
            # Update max_score if current_score is higher
            if current_score > max_score:
                max_score = current_score
    return max_score
```

#### Complexity Analysis:
- **Time Complexity:** O(n^2), due to the nested loop iterating through all pairs of points.
- **Space Complexity:** O(1), as we're using a constant amount of space.

---

### Approach 2: Optimized One Pass Calculation

#### Intuition:
We can reduce computations by understanding that we need to maximize `A[i] + i` for any subsequent element `A[j] - j` (where `j > i`). This allows us to compute the answer in a single pass through the array.

#### Approach:
1. Initialize `max_score` to 0 and `best_i` to `A[0] + 0`.
2. Traverse the array from the second element onwards.
3. For each element `A[j]`, calculate the potential sightseeing value using the formula `best_i + A[j] - j`.
4. Update `max_score` if the current value is higher.
5. Update `best_i` to `max(best_i, A[j] + j)` to ensure it represents the best `A[i] + i` seen so far as we iterate.

#### Code:
```python
def maxScoreSightseeingPair(A):
    max_score = 0
    best_i = A[0]  # Initially, consider the first element for i

    # Start from the second element
    for j in range(1, len(A)):
        # Calculate the current score for pair (best_i_candidate, A[j])
        max_score = max(max_score, best_i + A[j] - j)

        # Update best_i with the potential new value
        best_i = max(best_i, A[j] + j)

    return max_score
```

#### Complexity Analysis:
- **Time Complexity:** O(n), as we make a single pass through the array.
- **Space Complexity:** O(1), since we are using a constant amount of space to store variables. 

---

