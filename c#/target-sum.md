# [Leetcode 494: Target Sum](https://leetcode.com/problems/target-sum/)

## Approaches
- [Approach 1: Recursive Backtracking (Brute Force)](#approach-1)
- [Approach 2: Memoization (Top-Down DP)](#approach-2)
- [Approach 3: Dynamic Programming (Bottom-Up DP)](#approach-3)

## Approach 1: Recursive Backtracking (Brute Force)

### Intuition:
The problem can be visualized as adding either a positive or negative sign to each number in the array to reach a specific target sum. A basic way to solve this is by exploring all possible combinations using recursion. At each step, we decide whether to add or subtract the current number. This approach essentially generates a binary tree of possibilities for each number, leading to a significant computational cost due to overlapping subproblems.

### Approach:
1. Use a recursive function that considers both adding and subtracting the current number.
2. Base case: When we have processed all numbers, check if the current sum equals the target.
3. Recurse by including the current element as both positive and negative.
4. Sum counts of both recursive calls to get total number of ways.

```csharp
public class Solution {
    public int FindTargetSumWays(int[] nums, int target) {
        return Backtrack(nums, target, 0, 0);
    }

    private int Backtrack(int[] nums, int target, int index, int currentSum) {
        // Base case: Check if we are at the end of nums
        if (index == nums.Length) {
            // Check if the current sum is the target
            return currentSum == target ? 1 : 0;
        }
        
        // Recursive case: Count both positive and negative cases
        int add = Backtrack(nums, target, index + 1, currentSum + nums[index]);
        int subtract = Backtrack(nums, target, index + 1, currentSum - nums[index]);
        
        return add + subtract; // Sum the results from both branches
    }
}
```

**Time Complexity:** O(2^n), where n is the length of the array. Each number can be either positive or negative, creating 2^n combinations.

**Space Complexity:** O(n) due to the recursive call stack.

## Approach 2: Memoization (Top-Down DP)

### Intuition:
To optimize the recursive solution, we can use memoization to store results of subproblems that have already been computed. In this way, we avoid recomputing results for the same index and current sum combination.

### Approach:
1. Introduce a dictionary to store and reuse solutions for subproblems.
2. Key the subproblem solutions by the index and current sum.
3. Recursively calculate solutions only if they haven't been computed before.

```csharp
public class Solution {
    public int FindTargetSumWays(int[] nums, int target) {
        var memo = new Dictionary<(int, int), int>();
        return Helper(nums, target, 0, 0, memo);
    }

    private int Helper(int[] nums, int target, int index, int currentSum, Dictionary<(int, int), int> memo) {
        // Base case handling
        if (index == nums.Length) {
            return currentSum == target ? 1 : 0;
        }
        
        // Check if already computed
        if (memo.ContainsKey((index, currentSum))) {
            return memo[(index, currentSum)];
        }
        
        // Calculate both branches
        int add = Helper(nums, target, index + 1, currentSum + nums[index], memo);
        int subtract = Helper(nums, target, index + 1, currentSum - nums[index], memo);
        
        // Store the result in memo
        memo[(index, currentSum)] = add + subtract;
        
        return memo[(index, currentSum)];
    }
}
```

**Time Complexity:** O(n * sum), where n is the length of the array and sum is the range of possible sums.

**Space Complexity:** O(n * sum) due to the memoization dictionary and recursive stack.

## Approach 3: Dynamic Programming (Bottom-Up DP)

### Intuition:
Transform the recursive problem into a DP table that systematically builds up solutions from smaller subproblems, culminating in the solution for the entire problem. We approach this by dynamically generating possible sums for each number, updating a DP dictionary that maps sums to possible ways to achieve them. This reduces the problem into a variant of the subset sum problem.

### Approach:
1. Use a dictionary to record number of ways to achieve each sum for each number processed.
2. Start by initializing the base case.
3. Iterate each number in the input, updating potential sums in the DP table.
4. Return the number of ways to achieve the target sum stored in final DP state.

```csharp
public class Solution {
    public int FindTargetSumWays(int[] nums, int target) {
        var dp = new Dictionary<int, int>();
        dp[0] = 1; // Base case: one way to make zero sum with no numbers
        
        foreach (var num in nums) {
            var current = new Dictionary<int, int>();
            
            foreach (var entry in dp) {
                int sum = entry.Key;
                int count = entry.Value;
                
                // Add current number to the sum
                int addSum = sum + num;
                if (!current.ContainsKey(addSum)) current[addSum] = 0;
                current[addSum] += count;
                
                // Subtract current number from the sum
                int subtractSum = sum - num;
                if (!current.ContainsKey(subtractSum)) current[subtractSum] = 0;
                current[subtractSum] += count;
            }
            dp = current; // Move to next state
        }
        
        return dp.ContainsKey(target) ? dp[target] : 0;
    }
}
```

**Time Complexity:** O(n * sum), similar to the memoization approach, as all possible partial sums are calculated.

**Space Complexity:** O(sum), the DP dictionary only needs to store achievable sums for one pass through the numbers.

