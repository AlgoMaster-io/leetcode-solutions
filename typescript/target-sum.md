# [494. Target Sum](https://leetcode.com/problems/target-sum/)

## Approaches
1. [Brute Force Recursion](#approach-1-brute-force-recursion)
2. [Memoization](#approach-2-memoization)
3. [Dynamic Programming](#approach-3-dynamic-programming)

---

### Approach 1: Brute Force Recursion

#### Intuition:
The simplest way to solve this problem is using recursion. We attempt to decide on a sign (`+` or `-`) for each number in the array, trying every possible combination to reach the target sum. This attempts every possible combination of signs, which leads to an exponential number of possibilities.

#### Code:
```typescript
function findTargetSumWays(nums: number[], target: number): number {
    // Define a helper recursive function
    function calculateSum(index: number, currentSum: number): number {
        // Base case: if we've processed all numbers, check if we've reached the target sum
        if (index === nums.length) {
            return currentSum === target ? 1 : 0;
        }
        // Recurse by including the current number with both + and - signs
        return calculateSum(index + 1, currentSum + nums[index]) + 
               calculateSum(index + 1, currentSum - nums[index]);
    }
    return calculateSum(0, 0);
}
```

#### Time Complexity:
- Exponential, \(O(2^n)\) where \(n\) is the length of the input array.

#### Space Complexity:
- \(O(n)\) for the recursive call stack.

---

### Approach 2: Memoization

#### Intuition:
It's evident that the brute force solution recalculates the same states multiple times. By caching the results of these calculations, we can avoid redundant work, thus optimizing our recursive solution via memoization.

#### Code:
```typescript
function findTargetSumWays(nums: number[], target: number): number {
    // Create a map to cache computed results for index and currentSum
    const memo = new Map<string, number>();

    function calculateSum(index: number, currentSum: number): number {
        const cacheKey = `${index},${currentSum}`;
        // Return cached result if it exists
        if (memo.has(cacheKey)) {
            return memo.get(cacheKey)!;
        }

        // Base case: all numbers are processed
        if (index === nums.length) {
            return currentSum === target ? 1 : 0;
        }

        // Recursive exploration
        const add = calculateSum(index + 1, currentSum + nums[index]);
        const subtract = calculateSum(index + 1, currentSum - nums[index]);

        memo.set(cacheKey, add + subtract);
        return add + subtract;
    }
    return calculateSum(0, 0);
}
```

#### Time Complexity:
- \(O(n \times \sum)\) where \(n\) is the number of elements and \(\sum\) is the sum of all elements, since memoization reduces repeated subproblems.

#### Space Complexity:
- \(O(n \times \sum)\) for storing intermediate results in the cache.

---

### Approach 3: Dynamic Programming

#### Intuition:
The essence of this problem can be related to finding a subset of elements that add up to a given sum (subset sum problem). By transforming the problem into subsets and using dynamic programming, we can compute the number of ways to partition the set.

#### Conversion and Code:
To transform the problem: If `P` is the sum of elements with `+` and `N` is the sum of elements with `-`, then:
- `P - N = target`
- `P + N = sum(nums)`
From these, we solve for `P = (target + sum(nums)) / 2`, which necessitates that the sum is even. We then approach this as a subset sum problem.

```typescript
function findTargetSumWays(nums: number[], target: number): number {
    const totalSum = nums.reduce((acc, curr) => acc + curr, 0);
    // Check if target is achievable
    if ((target + totalSum) % 2 !== 0 || totalSum < Math.abs(target)) {
        return 0;
    }
    const subsetSum = (target + totalSum) / 2;
    
    // Create a dp array to store the number of ways for each sum
    const dp = new Array(subsetSum + 1).fill(0);
    dp[0] = 1;
    
    for (const num of nums) {
        for (let j = subsetSum; j >= num; j--) {
            dp[j] += dp[j - num];
        }
    }
    
    return dp[subsetSum];
}
```

#### Time Complexity:
- \(O(n \times \text{subsetSum})\), with `n` being the length of the input array.

#### Space Complexity:
- \(O(\text{subsetSum})\) for the DP array.

---

These approaches progressively improve from brute force to more efficient solutions leveraging memoization and dynamic programming, optimizing both time and space, especially for larger input sizes.

