# [Leetcode 416: Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Approaches:
1. [Recursive Backtracking](#recursive-backtracking)
2. [Dynamic Programming with Stateful DP Array](#dynamic-programming-with-stateful-dp-array)
3. [Dynamic Programming with Space Optimization](#dynamic-programming-with-space-optimization)

---

## Recursive Backtracking

### Intuition:
The problem can be conceptualized as finding a subset of numbers in the array such that the sum of the subset equals half of the total sum of the array. This leads to a recursive approach where, at each step, we decide whether to include a number in the current subset or not. 

### Code:
```csharp
public bool CanPartition(int[] nums) {
    int totalSum = nums.Sum();
    
    // If the total sum is odd, it's not possible to partition into two equal subsets
    if (totalSum % 2 != 0) return false;
    
    int subsetSum = totalSum / 2;
    return CanPartitionRecursive(nums, subsetSum, 0);
}

private bool CanPartitionRecursive(int[] nums, int remainingSum, int currentIndex) {
    // Base cases
    if (remainingSum == 0) return true; // Found a subset with the required sum
    if (currentIndex >= nums.Length) return false; // Exhausted all items
    
    // Recursive case: Check if including the current index can help reach the target sum
    if (nums[currentIndex] <= remainingSum) {
        if (CanPartitionRecursive(nums, remainingSum - nums[currentIndex], currentIndex + 1)) 
            return true;
    }
    
    // Recursive case: Exclude the current number and move to the next
    return CanPartitionRecursive(nums, remainingSum, currentIndex + 1);
}
```

### Complexity:
- **Time Complexity**: \(O(2^n)\) where n is the number of elements in the input array. Each element can either be included or excluded.
- **Space Complexity**: \(O(n)\). The depth of the recursive call stack.

---

## Dynamic Programming with Stateful DP Array

### Intuition:
We can improve on the recursive approach by using dynamic programming to eliminate redundant calculations. The idea is to use a boolean DP array where `dp[i]` indicates whether a subset with sum `i` can be achieved using the numbers seen so far.

### Code:
```csharp
public bool CanPartition(int[] nums) {
    int totalSum = nums.Sum();
    
    if (totalSum % 2 != 0) return false;
    
    int subsetSum = totalSum / 2;
    bool[,] dp = new bool[nums.Length + 1, subsetSum + 1];
    
    // Initialize: A subset sum of 0 is always possible
    for (int i = 0; i <= nums.Length; i++) {
        dp[i, 0] = true;
    }
    
    for (int i = 1; i <= nums.Length; i++) {
        for (int j = 1; j <= subsetSum; j++) {
            // Exclude the current number
            dp[i, j] = dp[i - 1, j];
            
            // Include the current number if possible
            if (nums[i - 1] <= j) {
                dp[i, j] = dp[i, j] || dp[i - 1, j - nums[i - 1]];
            }
        }
    }
    
    return dp[nums.Length, subsetSum];
}
```

### Complexity:
- **Time Complexity**: \(O(n \times \text{sum})\) where n is the number of elements and sum is total/2.
- **Space Complexity**: \(O(n \times \text{sum})\). Space for the DP table.

---

## Dynamic Programming with Space Optimization

### Intuition:
The DP solution can be further optimized to use only a 1D array. Since each DP state only depends on the previous row, we can carry over values from the previous iteration without needing a full 2D table.

### Code:
```csharp
public bool CanPartition(int[] nums) {
    int totalSum = nums.Sum();
    
    if (totalSum % 2 != 0) return false;
    
    int subsetSum = totalSum / 2;
    bool[] dp = new bool[subsetSum + 1];
    
    dp[0] = true; // A sum of 0 is always possible
    
    foreach (int num in nums) {
        for (int j = subsetSum; j >= num; j--) {
            // Update the dp table backwards to avoid overwriting
            dp[j] = dp[j] || dp[j - num];
        }
    }
    
    return dp[subsetSum];
}
```

### Complexity:
- **Time Complexity**: \(O(n \times \text{sum})\).
- **Space Complexity**: \(O(\text{sum})\). Space reduced by using a single-dimensional DP array.

These solutions highlight a range of approaches from simple recursion to optimized dynamic programming, each progressively reducing the computational overhead.

