# [Leetcode 213: House Robber II](https://leetcode.com/problems/house-robber-ii/)

## Approaches
1. [Recursive Brute Force](#recursive-brute-force)
2. [Dynamic Programming with Memoization](#dynamic-programming-with-memoization)
3. [Optimized Dynamic Programming](#optimized-dynamic-programming)

## Recursive Brute Force

### Intuition
In the House Robber II problem, the houses are arranged in a circle. This means we cannot simply apply the previous House Robber solution as-is due to the circular constraint. We should consider two cases: one where the first house is robbed and the other where the last house is robbed. By breaking the problem into these two cases, we can apply the same recursive strategy used in the original problem individually to both cases.

### Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int rob_helper(std::vector<int>& nums, int start, int end) {
    if (start > end) return 0;

    // Choose between robbing current house vs moving to the next
    int rob_house = nums[start] + rob_helper(nums, start + 2, end);
    int skip_house = rob_helper(nums, start + 1, end);
    
    return std::max(rob_house, skip_house);
}

int rob(std::vector<int>& nums) {
    if (nums.empty()) return 0;
    if (nums.size() == 1) return nums[0];
    
    // Compare results from the two scenarios
    return std::max(rob_helper(nums, 0, nums.size() - 2), rob_helper(nums, 1, nums.size() - 1));
}
```

### Complexity
- **Time Complexity**: `O(2^n)`, due to exhaustive exploration of all possible paths.
- **Space Complexity**: `O(n)`, accounting for the recursion call stack.

## Dynamic Programming with Memoization

### Intuition
We can improve on the recursive strategy by using memoization to store intermediate results, thus avoiding redundant calculations. By storing the results of subproblems, we prevent recomputation and efficiently compute the solution by considering two sub-cases in a circular arrangement.

### Code
```cpp
#include <vector>
#include <algorithm>

int rob_helper(std::vector<int>& nums, int start, int end, std::vector<int>& memo) {
    if (start > end) return 0;
    if (memo[start] != -1) return memo[start];

    // Store in memo the solution to subproblem
    memo[start] = std::max(nums[start] + rob_helper(nums, start + 2, end, memo), 
                           rob_helper(nums, start + 1, end, memo));
    return memo[start];
}

int rob(std::vector<int>& nums) {
    if (nums.empty()) return 0;
    if (nums.size() == 1) return nums[0];

    std::vector<int> memo1(nums.size(), -1);
    std::vector<int> memo2(nums.size(), -1);

    int rob_first = rob_helper(nums, 0, nums.size() - 2, memo1);
    int rob_last = rob_helper(nums, 1, nums.size() - 1, memo2);

    return std::max(rob_first, rob_last);
}
```

### Complexity
- **Time Complexity**: `O(n)`, we solve each subproblem once.
- **Space Complexity**: `O(n)`, for storing memoization arrays.

## Optimized Dynamic Programming

### Intuition
To further optimize, we can use a bottom-up dynamic programming approach with constant space. This eliminates the need for the memo array, as each subproblem only depends on the two previous ones. By iterating over the circular cases separately, we maintain optimal substructure efficiently.

### Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int rob_linear(std::vector<int>& nums, int start, int end) {
    int prev1 = 0, prev2 = 0;
    for (int i = start; i <= end; ++i) {
        int temp = std::max(prev1, prev2 + nums[i]);
        prev2 = prev1;
        prev1 = temp;
    }
    return prev1;
}

int rob(std::vector<int>& nums) {
    if (nums.empty()) return 0;
    if (nums.size() == 1) return nums[0];
    
    // Compare results between the two configurations
    return std::max(rob_linear(nums, 0, nums.size() - 2), rob_linear(nums, 1, nums.size() - 1));
}
```

### Complexity
- **Time Complexity**: `O(n)`, each element is processed a constant number of times.
- **Space Complexity**: `O(1)`, we use only constant extra space for storing the last two state values.

