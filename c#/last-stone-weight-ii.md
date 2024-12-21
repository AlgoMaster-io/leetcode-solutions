# [Leetcode 1049: Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Approaches:
1. [Recursive Solution without Memoization](#approach-1)
2. [Recursive Solution with Memoization](#approach-2)
3. [Dynamic Programming (Subset Sum Problem)](#approach-3)

---

## Approach 1: Recursive Solution without Memoization

### Intuition:
The problem can be viewed as a classic subset partition problem where we want to partition the array into two subsets such that the difference between the sum of these two subsets is minimized. The most basic approach is to recursively explore all possible partitions and calculate the minimum difference.

### Steps:
1. Define a recursive function that tries to assign each stone to one of the two sets.
2. Compute the difference of weights between the two sets once the end of the array is reached.
3. Return the minimum difference obtained from the recursive exploration.

### Code:
```csharp
public class Solution {
    public int LastStoneWeightII(int[] stones) {
        return PartitionHelper(stones, 0, 0, 0);
    }
    
    // Recursive function to calculate the minimum difference
    private int PartitionHelper(int[] stones, int index, int sum1, int sum2) {
        // When all stones are processed, return the absolute difference of sums
        if (index == stones.Length) {
            return Math.Abs(sum1 - sum2);
        }
        
        // Explore both assigning current stone to the first subset and the second subset
        int diff1 = PartitionHelper(stones, index + 1, sum1 + stones[index], sum2);
        int diff2 = PartitionHelper(stones, index + 1, sum1, sum2 + stones[index]);
        
        // Return the minimum difference from both choices
        return Math.Min(diff1, diff2);
    }
}
```

### Time Complexity:
O(2^n) - Since each stone can either go into one of two subsets, leading to an exponential number of possibilities.

### Space Complexity:
O(n) - The depth of recursive calls is proportional to the number of stones, leading to linear space due to the function call stack.

---

## Approach 2: Recursive Solution with Memoization

### Intuition:
To improve upon the basic recursive solution, we can use memoization to store and reuse results of previously computed states, reducing the time complexity significantly.

### Steps:
1. Use a dictionary to store results of previously computed states based on current index and sum differences.
2. In each recursive call, check if the result for the current state has already been calculated and stored.
3. Reuse the stored result if available, otherwise, compute and store the result.

### Code:
```csharp
public class Solution {
    private Dictionary<string, int> memo;

    public int LastStoneWeightII(int[] stones) {
        memo = new Dictionary<string, int>();
        return PartitionHelper(stones, 0, 0, 0);
    }
    
    private int PartitionHelper(int[] stones, int index, int sum1, int sum2) {
        if (index == stones.Length) {
            return Math.Abs(sum1 - sum2);
        }
        
        string key = $"{index}-{Math.Abs(sum1 - sum2)}";
        
        if (memo.ContainsKey(key)) {
            return memo[key];
        }
        
        int diff1 = PartitionHelper(stones, index + 1, sum1 + stones[index], sum2);
        int diff2 = PartitionHelper(stones, index + 1, sum1, sum2 + stones[index]);
        
        int result = Math.Min(diff1, diff2);
        memo[key] = result;
        return result;
    }
}
```

### Time Complexity:
O(n * S) - Here S is the range of subset sums. Memoization avoids recomputation, making it more efficient.

### Space Complexity:
O(n * S) - Storing results for each state in the dictionary and the recursive stack depth.

---

## Approach 3: Dynamic Programming (Subset Sum Problem)

### Intuition:
This problem is a variant of the "Subset Sum Problem." The idea is to find the largest sum closest to `total_sum/2`. This can be solved using a dynamic programming approach where we build up a solution iteratively.

### Steps:
1. Calculate the total sum of all stones.
2. Use a boolean DP array to track which possible sums can be achieved using subsets of the stones.
3. Transition the DP array by updating which sums are possible with each additional stone.
4. Find the closest possible sum to `total_sum/2` and calculate the result based on this.

### Code:
```csharp
public class Solution {
    public int LastStoneWeightII(int[] stones) {
        int totalSum = stones.Sum();
        int target = totalSum / 2;
        bool[] dp = new bool[target + 1];
        dp[0] = true;
        
        foreach (int stone in stones) {
            for (int j = target; j >= stone; j--) {
                dp[j] = dp[j] | dp[j - stone];
            }
        }
        
        for (int j = target; j >= 0; j--) {
            if (dp[j]) {
                return totalSum - 2 * j;
            }
        }
        
        return totalSum;
    }
}
```

### Time Complexity:
O(n * S/2) - Where n is the number of stones and S is the sum of all stone weights. We compute possible sums up to half the total sum.

### Space Complexity:
O(S/2) - Storing the boolean DP array for possible sums up to half of the total sum. 

---

By employing these different approaches, one can solve the problem with varying degrees of efficiency, using either recursion or dynamic programming techniques.

