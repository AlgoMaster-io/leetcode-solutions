# 746. [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approach 1: Recursion with Memoization

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        // Create a memoization array for storing results of subproblems
        vector<int> memo(cost.size(), 0);
        return min(minCost(cost.size() - 1, cost, memo),
                   minCost(cost.size() - 2, cost, memo));
    }

private:
    int minCost(int i, const vector<int>& cost, vector<int>& memo) {
        if (i < 0) return 0; // Base case: No cost for negative steps
        if (memo[i] != 0) return memo[i]; // Return precomputed cost if available

        // Minimum cost to reach this step
        int costToReach = cost[i] + min(minCost(i - 1, cost, memo), 
                                        minCost(i - 2, cost, memo));
        
        memo[i] = costToReach; // Store computed result in memo array
        return costToReach;
    }
};
```

## Approach 2: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        vector<int> dp(n + 1, 0);
        
        // Base cases: No cost to start at either step 0 or step 1
        dp[0] = 0;
        dp[1] = 0;

        // Fill the dp array with minimum cost to reach each step
        for (int i = 2; i <= n; i++) {
            dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }

        return dp[n]; // Minimum cost to reach the top
    }
};
```

## Approach 3: Dynamic Programming with Constant Space

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        int prev1 = 0;
        int prev2 = 0;

        // Iterate through cost array and calculate minimum cost dynamically
        for (int i = 2; i <= n; i++) {
            int current = min(prev1 + cost[i - 1], prev2 + cost[i - 2]);
            prev2 = prev1;
            prev1 = current;
        }

        return prev1; // Minimum cost to reach the top
    }
};
```

