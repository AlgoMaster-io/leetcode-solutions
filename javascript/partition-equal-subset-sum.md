# [Leetcode 416: Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Approaches
- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

### Approach 1: Recursion

The basic idea to solve this problem is to find if there's a subset in the array whose sum is equal to half of the total sum of the array. If such a subset exists, the other half will automatically equal the first half.

#### Intuition
1. Calculate the total sum of the array. If it is odd, then it's impossible to split it into two equal sums.
2. Use a recursive function to iterate through the array and check both possibilities for each number: including it in the subset or not.
3. If at any point the sum of the included numbers equals half of the total sum, return true.

#### Code

```javascript
function canPartition(nums) {
    function canFindSum(index, currentSum) {
        // Base condition: if the current sum is exactly half, we found a subset
        if (currentSum === halfSum) return true;
        // If we exceed half, or run out of numbers, return false
        if (currentSum > halfSum || index >= nums.length) return false;
        // Recursively check both possibilities: include or not include the number
        return canFindSum(index + 1, currentSum + nums[index]) || canFindSum(index + 1, currentSum);
    }
    
    const totalSum = nums.reduce((acc, num) => acc + num, 0);
    // If total sum is odd, partitioning equally is impossible
    if (totalSum % 2 !== 0) return false;
    
    const halfSum = totalSum / 2;
    return canFindSum(0, 0);
}
```

#### Time and Space Complexity
- **Time Complexity**: O(2^n) - The recursive tree grows exponentially with two branch possibilities at each step.
- **Space Complexity**: O(n) - The recursive call stack can grow up to the size of the number of elements in the array.

### Approach 2: Recursion with Memoization

To optimize the recursive approach, one can naturally use memoization, storing already computed combination results to avoid recomputation.

#### Intuition
This approach builds upon the first one by adding a caching mechanism to store results of previous calculations.

#### Code

```javascript
function canPartition(nums) {
    const totalSum = nums.reduce((acc, num) => acc + num, 0);
    if (totalSum % 2 !== 0) return false; // If total sum is odd, partitioning equally is impossible.
    
    const halfSum = totalSum / 2;
    const memo = new Map();
    
    function canFindSum(index, currentSum) {
        const key = `${index}-${currentSum}`;
        if (memo.has(key)) return memo.get(key);
        if (currentSum === halfSum) return true;
        if (currentSum > halfSum || index >= nums.length) return false;

        const found = canFindSum(index + 1, currentSum + nums[index]) || canFindSum(index + 1, currentSum);
        memo.set(key, found);
        return found;
    }

    return canFindSum(0, 0);
}
```

#### Time and Space Complexity
- **Time Complexity**: O(n * halfSum) - Each state is computed once and stored in the memo.
- **Space Complexity**: O(n * halfSum) - Space required for memoization.

### Approach 3: Dynamic Programming

The most optimal solution uses dynamic programming to solve the subset sum problem.

#### Intuition
1. Use a boolean DP array where `dp[j]` indicates whether a subset sum of `j` is possible with the available numbers.
2. Initialize `dp[0]` as true since a sum of zero can always be made with an empty subset.
3. Iterating through each number, update the DP array in reverse to prevent using the same number twice for the current state.

#### Code

```javascript
function canPartition(nums) {
    const totalSum = nums.reduce((acc, num) => acc + num, 0);
    // If total sum is odd, partitioning equally is impossible
    if (totalSum % 2 !== 0) return false;
    
    const halfSum = totalSum / 2;
    const dp = new Array(halfSum + 1).fill(false);
    dp[0] = true;

    for (let num of nums) {
        // Traverse backwards to prevent reusing the number
        for (let j = halfSum; j >= num; j--) {
            // Update dp[j] to true if the current number is used 
            // and adds up to a valid subset sum
            dp[j] = dp[j] || dp[j - num];
        }
    }

    return dp[halfSum];
}
```

#### Time and Space Complexity
- **Time Complexity**: O(n * halfSum) - We fill out the DP array of size halfSum `n` times.
- **Space Complexity**: O(halfSum) - The size of the DP array is proportional to half of the total sum.

