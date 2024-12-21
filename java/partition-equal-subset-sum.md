# [Leetcode 416: Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Solutions
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)

## Recursive Approach

### Intuition
The problem can be seen as a variation of the 0/1 knapsack problem. The most basic approach is to use recursion to try every possible subset and check if there is a subset with a sum that is equal to half of the total sum of the array. We are using a recursive function that tries to include or exclude each number.

### Code
```java
public boolean canPartition(int[] nums) {
    int totalSum = 0;
    for (int num : nums) totalSum += num;
    
    // If the total sum is odd, we can't possibly split it into two equal subsets.
    if (totalSum % 2 != 0) return false;
    
    return canPartitionRecursive(nums, totalSum / 2, 0);
}

private boolean canPartitionRecursive(int[] nums, int sum, int currentIndex) {
    // Base case: if sum is zero, we found a subset
    if (sum == 0) return true;
    
    // Base case: if no more elements or sum < 0
    if (nums.length == 0 || currentIndex >= nums.length) return false;
    
    // Recursive call after choosing the number at the currentIndex
    // if the number can be part of the solution
    if (nums[currentIndex] <= sum) {
        if (canPartitionRecursive(nums, sum - nums[currentIndex], currentIndex + 1)) {
            return true;
        }
    }

    // Recursive call after excluding the number at the currentIndex
    return canPartitionRecursive(nums, sum, currentIndex + 1);
}
```

### Time Complexity
- O(2^n) - where `n` is the number of elements in the input array. This is due to the fact we have two choices (include/exclude) for each number.

### Space Complexity
- O(n) - due to the recursion stack used in recursive calls.

## Memoization Approach

### Intuition
The recursive approach has an exponential time complexity because it recalculates results for the same subproblems multiple times. We can optimize it by storing intermediate results in a memoization table.

### Code
```java
public boolean canPartition(int[] nums) {
    int totalSum = 0;
    for (int num : nums) totalSum += num;
    
    if (totalSum % 2 != 0) return false;
    
    Boolean[][] dp = new Boolean[nums.length][totalSum / 2 + 1];
    return canPartitionMemoize(nums, totalSum / 2, 0, dp);
}

private boolean canPartitionMemoize(int[] nums, int sum, int currentIndex, Boolean[][] dp) {
    if (sum == 0) return true;
    if (nums.length == 0 || currentIndex >= nums.length) return false;
    
    if (dp[currentIndex][sum] != null) {
        return dp[currentIndex][sum];
    }

    if (nums[currentIndex] <= sum && canPartitionMemoize(nums, sum - nums[currentIndex], currentIndex + 1, dp)) {
        dp[currentIndex][sum] = true;
        return true;
    }

    dp[currentIndex][sum] = canPartitionMemoize(nums, sum, currentIndex + 1, dp);
    return dp[currentIndex][sum];
}
```

### Time Complexity
- O(n * s) - `s` is half of the total sum.

### Space Complexity
- O(n * s) - for the memoization table, and O(n) for the recursion stack.

## Dynamic Programming Approach

### Intuition
We can solve the problem iteratively using dynamic programming with a boolean array. We aim to determine if we can form the subset with sum `totalSum/2`, by iteratively updating our possibilities in a boolean array.

### Code
```java
public boolean canPartition(int[] nums) {
    int totalSum = 0;
    for (int num : nums) totalSum += num;
    
    if (totalSum % 2 != 0) return false;
    
    int sum = totalSum / 2;
    boolean[] dp = new boolean[sum + 1];
    dp[0] = true; // Base case: zero sum is always achievable

    for (int num : nums) {
        for (int s = sum; s >= num; s--) {
            dp[s] = dp[s] || dp[s - num];
        }
    }
    return dp[sum];
}
```

### Time Complexity
- O(n * s) - where `n` is number of elements in the array and `s` is half the total sum of all elements.

### Space Complexity
- O(s) - this is space used by the `dp` array.

This is the most optimal solution in terms of both time and space complexity.

