# 46. [Permutations](https://leetcode.com/problems/permutations/)

## Approach 1: Brute Force (Generate All Permutations)

### Solution
```javascript
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n * n!) for storing the permutations
function permute(nums) {
    const result = [];
    const visited = Array(nums.length).fill(false);
    generateAll(nums, [], result, visited);
    return result;
}

function generateAll(nums, current, result, visited) {
    if (current.length === nums.length) {
        result.push([...current]); // Add the current permutation
        return;
    }

    for (let i = 0; i < nums.length; i++) {
        if (!visited[i]) {
            visited[i] = true;
            current.push(nums[i]); // Choose the current element
            generateAll(nums, current, result, visited); // Recurse
            current.pop(); // Backtrack
            visited[i] = false; // Reset visited state
        }
    }
}
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
```javascript
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n * n!) for storing the permutations
function permute(nums) {
    const result = [];
    backtrack(nums, [], result);
    return result;
}

function backtrack(nums, current, result) {
    if (current.length === nums.length) {
        result.push([...current]); // Add the current permutation
        return;
    }

    for (let i = 0; i < nums.length; i++) {
        if (current.includes(nums[i])) {
            continue; // Skip if the number is already in the current permutation
        }
        current.push(nums[i]); // Choose the current element
        backtrack(nums, current, result); // Recurse
        current.pop(); // Backtrack
    }
}
```

## Approach 3: Swap-Based Backtracking (In-Place Modification)

### Solution
```javascript
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n) for the recursion stack
function permute(nums) {
    const result = [];
    backtrack(nums, 0, result);
    return result;
}

function backtrack(nums, start, result) {
    if (start === nums.length) {
        result.push([...nums]); // Add the current permutation
        return;
    }

    for (let i = start; i < nums.length; i++) {
        swap(nums, start, i); // Swap to create a new permutation
        backtrack(nums, start + 1, result); // Recurse
        swap(nums, start, i); // Backtrack to the original state
    }
}

function swap(nums, i, j) {
    const temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp; // Swap two elements in the array
}
```

