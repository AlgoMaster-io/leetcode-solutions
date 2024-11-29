# 78. [Subsets](https://leetcode.com/problems/subsets/)

## Approach 1: Iterative (Power Set)

### Solution
```javascript
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n * 2^n) for storing the subsets
function subsets(nums) {
    const result = [[]]; // Start with the empty subset

    for (let num of nums) {
        const size = result.length;
        for (let i = 0; i < size; i++) {
            const subset = [...result[i]];
            subset.push(num); // Add the current number to each existing subset
            result.push(subset);
        }
    }

    return result;
}
```

## Approach 2: Backtracking (Recursive Subset Generation)

### Solution
```javascript
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n) for the recursion stack
function subsets(nums) {
    const result = [];
    backtrack(nums, 0, [], result);
    return result;
}

function backtrack(nums, index, current, result) {
    result.push([...current]); // Add the current subset to the result

    for (let i = index; i < nums.length; i++) {
        current.push(nums[i]); // Include nums[i] in the current subset
        backtrack(nums, i + 1, current, result); // Recurse with the next index
        current.pop(); // Backtrack to explore other subsets
    }
}
```

## Approach 3: Bitmasking (Iterative Subset Generation)

### Solution
```javascript
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n * 2^n) for storing the subsets
function subsets(nums) {
    const result = [];
    const totalSubsets = 1 << nums.length; // Total subsets = 2^n

    for (let mask = 0; mask < totalSubsets; mask++) {
        const subset = [];
        for (let i = 0; i < nums.length; i++) {
            if (mask & (1 << i)) {
                subset.push(nums[i]); // Include nums[i] in the subset
            }
        }
        result.push(subset);
    }

    return result;
}
```

