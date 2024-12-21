# [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approaches
1. [Recursive Approach with Memoization](#approach1)
2. [Dynamic Programming - Bottom-Up Approach](#approach2)
3. [Space Optimized Dynamic Programming](#approach3)

---

### Approach 1: Recursive Approach with Memoization<a id="approach1"></a>

#### Intuition:
This approach aims to calculate the minimum cost to reach the top of the stairs using recursion with memoization to store already computed values. For each step, we have the option to come from either the previous step or the step before that. By computing the minimum cost for each step and storing it, we can avoid redundant calculations.

#### Code:
```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    int minCost(std::vector<int>& cost, int i, std::vector<int>& memo) {
        // Base case: if out of bounds, return 0 (we've reached the top)
        if (i >= cost.size()) return 0;

        // Return the precomputed cost if available
        if (memo[i] != -1) return memo[i];

        // Calculate the minimum cost for this step
        int takeOneStep = minCost(cost, i + 1, memo) + cost[i];
        int takeTwoSteps = minCost(cost, i + 2, memo) + cost[i];

        // Store the result in memo array
        memo[i] = std::min(takeOneStep, takeTwoSteps);
        return memo[i];
    }
    
    int minCostClimbingStairs(std::vector<int>& cost) {
        std::vector<int> memo(cost.size(), -1);
        // Start from step 0 or step 1
        return std::min(minCost(cost, 0, memo), minCost(cost, 1, memo));
    }
};
```

#### Time Complexity:
- **O(n)**: Each step cost is calculated once due to memoization, with n being the number of steps.

#### Space Complexity:
- **O(n)**: Space is used for the memoization array.

---

### Approach 2: Dynamic Programming - Bottom-Up Approach<a id="approach2"></a>

#### Intuition:
We construct a dynamic programming (DP) array where each element represents the minimum cost to reach that step. Starting from the base cases (step 0 and step 1), we iteratively compute the cost for each step by considering the two possible previous steps.

#### Code:
```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    int minCostClimbingStairs(std::vector<int>& cost) {
        int n = cost.size();
        std::vector<int> dp(n + 1, 0);
        
        // Build the dp array bottom-up
        for (int i = 2; i <= n; ++i) {
            dp[i] = std::min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }
        
        // Minimum cost to reach step n
        return dp[n];
    }
};
```

#### Time Complexity:
- **O(n)**: We calculate the minimum cost for each step.

#### Space Complexity:
- **O(n)**: Space is used by the DP array.

---

### Approach 3: Space Optimized Dynamic Programming<a id="approach3"></a>

#### Intuition:
We can reduce the space usage by only storing the last two costs instead of all the costs to reach previous steps. By iterating through the cost array and keeping track of only the last two computed costs, we further optimize the space complexity.

#### Code:
```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    int minCostClimbingStairs(std::vector<int>& cost) {
        int n = cost.size();
        int prev1 = 0; // cost to reach step i - 1
        int prev2 = 0; // cost to reach step i - 2
        int current;

        for (int i = 2; i <= n; ++i) {
            current = std::min(prev1 + cost[i - 1], prev2 + cost[i - 2]);
            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
};
```

#### Time Complexity:
- **O(n)**: Each step is processed once.

#### Space Complexity:
- **O(1)**: Only a fixed amount of space is used.

