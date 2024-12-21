# [334. Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)

### Approaches:
1. [Brute Force](#brute-force)
2. [Optimized with Two Variables](#optimized-with-two-variables)

## Brute Force

### Intuition:
The simplest way to tackle this problem is to consider all possible subsequences of length 3. If we find any subsequence `(i, j, k)` such that `nums[i] < nums[j] < nums[k]`, we return true. This brute force approach involves three nested loops, which can be inefficient for large inputs.

### Approach:
- Use three nested loops to check every possible triplet.
- Compare each selected triplet to see if it satisfies the condition `nums[i] < nums[j] < nums[k]`.

### Code:
```javascript
var increasingTriplet = function(nums) {
    let n = nums.length;
    // Iterate through all combinations of i, j, k
    for (let i = 0; i < n - 2; i++) {
        for (let j = i + 1; j < n - 1; j++) {
            for (let k = j + 1; k < n; k++) {
                // Check if we have an increasing triplet
                if (nums[i] < nums[j] && nums[j] < nums[k]) {
                    return true;
                }
            }
        }
    }
    return false;
};
```

### Complexity:
- **Time Complexity**: \(O(n^3)\), where `n` is the length of the input array. This is due to the three nested loops.
- **Space Complexity**: \(O(1)\), since no additional space is used other than a few variables.

## Optimized with Two Variables

### Intuition:
A more efficient solution can be obtained by maintaining two variables `first` and `second` to track the smallest and the second smallest numbers encountered so far. The goal is to find three numbers such that they form an increasing order with the help of these two variables.

### Approach:
- Initialize `first` and `second` to infinity.
- Traverse through the array:
  - If the current number is smaller than `first`, update `first`.
  - Else if it is smaller than `second`, update `second`.
  - Otherwise, if there's a number greater than `second`, we've found our triplet.

### Code:
```javascript
var increasingTriplet = function(nums) {
    // Initialize the first and second variables to infinte
    let first = Infinity;
    let second = Infinity;

    // Iterate through the numbers in the array
    for (let num of nums) {
        if (num <= first) {
            // Update first to the smallest number found so far
            first = num;
        } else if (num <= second) {
            // Update second to the next smallest number after first
            second = num;
        } else {
            // If the current number is greater than both first and second,
            // we have found the triplet
            return true;
        }
    }
    return false;
};
```

### Complexity:
- **Time Complexity**: \(O(n)\), where `n` is the length of the input array. We traverse the array only once.
- **Space Complexity**: \(O(1)\), since only a constant amount of space is used by the variables `first` and `second`. 

This approach efficiently finds an increasing triplet in linear time, making it the optimal solution for this problem.

