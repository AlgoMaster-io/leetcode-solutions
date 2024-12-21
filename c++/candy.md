## [Leetcode Problem 135: Candy](https://leetcode.com/problems/candy/)

### Approaches:

- [Approach 1: Naive Brute Force](#approach-1-naive-brute-force)
- [Approach 2: Two-Pass Greedy Solution](#approach-2-two-pass-greedy-solution)

---

### Approach 1: Naive Brute Force

#### Intuition

The problem requires us to distribute candies to children seated in a line such that:
1. Each child must get at least one candy.
2. A child with a higher rating gets more candies than their neighbors.

A naive solution is to initialize an array with one candy for each child and then try to adjust the candy amounts to satisfy the conditions, iterating through the list and checking each child in relation to their neighbors.

#### Implementation

```cpp
int candy(vector<int>& ratings) {
    int n = ratings.size();
    vector<int> candies(n, 1);

    // Keep updating until all rules are satisfied
    bool updated = true;
    while (updated) {
        updated = false;
        for (int i = 0; i < n; i++) {
            if (i > 0 && ratings[i] > ratings[i-1] && candies[i] <= candies[i-1]) {
                candies[i] = candies[i-1] + 1;
                updated = true;
            }
            if (i < n-1 && ratings[i] > ratings[i+1] && candies[i] <= candies[i+1]) {
                candies[i] = candies[i+1] + 1;
                updated = true;
            }
        }
    }
    
    return accumulate(candies.begin(), candies.end(), 0);
}
```

#### Complexity Analysis

- **Time Complexity:** \(O(n^2)\) due to repeated iterations until no changes occur.
- **Space Complexity:** \(O(n)\) for storing the candies array.

---

### Approach 2: Two-Pass Greedy Solution

#### Intuition

For an optimal solution, leverage a greedy approach with two passes:
1. Traverse from left to right; each time a child's rating is higher than the previous one, they get more candies.
2. Traverse from right to left; similarly, adjust for each case where a child's rating is higher than the next one, ensuring the candy count is enough compared to both directions.

#### Implementation

```cpp
int candy(vector<int>& ratings) {
    int n = ratings.size();
    
    // Initialize candy distribution
    vector<int> candies(n, 1);

    // Left to right pass
    for (int i = 1; i < n; ++i) {
        if (ratings[i] > ratings[i-1]) {
            candies[i] = candies[i-1] + 1;
        }
    }

    // Right to left pass
    for (int i = n-2; i >= 0; --i) {
        if (ratings[i] > ratings[i+1]) {
            candies[i] = max(candies[i], candies[i+1] + 1);
        }
    }

    // Sum up the candies
    return accumulate(candies.begin(), candies.end(), 0);
}
```

#### Complexity Analysis

- **Time Complexity:** \(O(n)\). Each pass through the ratings is linear.
- **Space Complexity:** \(O(n)\) for the candies array.

This two-pass solution dramatically reduces the complexity compared to the naive approach and is optimal given the problem constraints.

