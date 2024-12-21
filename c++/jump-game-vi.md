# [Leetcode 1696: Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Table of Contents
1. [Approach 1: Recursion with Memoization](#approach-1-recursion-with-memoization)
2. [Approach 2: Dynamic Programming with Deque for Optimization](#approach-2-dynamic-programming-with-deque-for-optimization)

---

## Approach 1: Recursion with Memoization

### Intuition
The problem can be approached by using a recursive strategy where for each position `i` in the `nums` array, we calculate the maximum score that can be obtained starting from position `i`. Since there is overlap in the subproblems, a direct recursive solution would involve a lot of repeated calculations. Therefore, it is more efficient to use memoization to store the results of previously computed subproblems.

### Detailed Steps
1. Define a recursive function `solve` that computes the maximum score starting from a given index.
2. Use memoization to cache the results of computed indices.
3. For each index `i`, recursively calculate the maximum score obtainable by jumping to positions within the allowed jump range `[i+1, i+k]`.
4. Use these calculated scores to determine the best option for the current index `i`.
5. Sum the current position `nums[i]` with the result obtained from the recursive calls to get the total score from index `i`.

### Time Complexity
- **O(n * k)**: Each of the `n` positions will potentially involve up to `k` recursive calls.

### Space Complexity
- **O(n)**: Due to the use of memoization cache.

```cpp
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> memo(n, INT_MIN);
        
        // Recursive function with memoization
        function<int(int)> solve = [&](int i) -> int {
            // Base case: If we reach the last index
            if (i == n - 1) return nums[i];
            
            // Return already solved subproblem
            if (memo[i] != INT_MIN) return memo[i];
            
            // Calculate the max score from current position
            int max_score = INT_MIN;
            for (int j = 1; j <= k && i + j < n; ++j) {
                max_score = max(max_score, solve(i + j));
            }
            
            return memo[i] = nums[i] + max_score;
        };
        
        return solve(0);
    }
};
```

---

## Approach 2: Dynamic Programming with Deque for Optimization

### Intuition
Building on the insights from the first approach, we can further optimize by realizing that when jumping within the constraint `k`, only the best previous scores within the `k` window influence the current score. By leveraging a double-ended queue (deque), we can maintain a window of optimal scores efficiently, thus ensuring that each element in the window is in a decreasing order based on scores.

### Detailed Steps
1. Use a deque to store indices in the decreasing order of their scores calculated from a DP list.
2. Initialize the DP list where `dp[i]` holds the maximum score to reach the end starting from index `i`.
3. Iterate through the `nums` array.
4. For each index, update the DP table by adding `nums[i]` to `dp` value of the index at the front of the deque.
5. Maintain the order of the deque and ensure that elements providing lower scores than the current element are removed.
6. Ensure elements older than `k` from the current index are also removed from the deque.
7. Return the DP value at the starting index which is the maximum obtainable score from the start to the end of the array.

### Time Complexity
- **O(n)**: Each element is added and removed from the deque at most once.

### Space Complexity
- **O(n)**: For the DP array and additionally the space used by the deque.

```cpp
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();
        deque<int> dq;
        vector<int> dp(n);
        
        dp[0] = nums[0];
        dq.push_back(0); // The starting index

        for (int i = 1; i < n; ++i) {
            // Remove indexes that are out of range [i-k, i]
            while (!dq.empty() && dq.front() < i - k) {
                dq.pop_front();
            }
            
            // The element at the front of the deque is the optimal previous index
            dp[i] = nums[i] + dp[dq.front()];
            
            // Maintain deque in decreasing order of dp values
            while (!dq.empty() && dp[i] >= dp[dq.back()]) {
                dq.pop_back();
            }
            
            // Add current index to the deque
            dq.push_back(i);
        }
        
        return dp[n - 1]; // Maximum score to arrive at the last index
    }
};
```


