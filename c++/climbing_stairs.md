# 70. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

## Approach 1: Recursion with Memoization

### Solution
c++
// Time Complexity: O(n)
// Space Complexity: O(n)
```cpp
#include <unordered_map>

class Solution {
private:
    std::unordered_map<int, int> memo;

public:
    int climbStairs(int n) {
        if (n <= 2) return n;
        if (memo.find(n) != memo.end()) return memo[n];
        
        // Recurrence relation: f(n) = f(n-1) + f(n-2)
        int result = climbStairs(n - 1) + climbStairs(n - 2);
        memo[n] = result; // Store the result in a map to avoid recalculating
        return result;
    }
};
```

## Approach 2: Dynamic Programming

### Solution
c++
// Time Complexity: O(n)
// Space Complexity: O(n)
```cpp
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2) return n;

        int dp[n + 1];
        dp[1] = 1;
        dp[2] = 2;

        // Build the dp array with previous solutions
        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n]; // The answer is stored in the last element
    }
};
```

## Approach 3: Optimized Dynamic Programming

### Solution
c++
// Time Complexity: O(n)
// Space Complexity: O(1)
```cpp
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2) return n;

        int first = 1;  // Represents dp[i-2]
        int second = 2; // Represents dp[i-1]
        int current = 0;

        // Use two variables to avoid full dp array and update them iteratively
        for (int i = 3; i <= n; i++) {
            current = first + second; // Current step number is sum of previous two steps
            first = second; // Move second to first
            second = current; // Update second to the current
        }

        return current; // Result is stored in current
    }
};
```
