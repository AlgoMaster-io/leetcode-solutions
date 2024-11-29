# 837. [New 21 Game](https://leetcode.com/problems/new-21-game/)

## Approach 1: Dynamic Programming with Probability Calculation

### Solution
```cpp
// Time Complexity: O(n + k)
// Space Complexity: O(n)
class Solution {
public:
    double new21Game(int n, int k, int maxPts) {
        // Edge case: if k is 0, Alice can stop immediately with a probability of 1
        if (k == 0 || n >= k + maxPts) return 1.0;

        // dp[i] is the probability of reaching point i
        vector<double> dp(n + 1, 0.0);
        dp[0] = 1.0;

        double windowSum = 1.0; // Sum of probabilities in the window
        double result = 0.0;

        for (int i = 1; i <= n; i++) {
            dp[i] = windowSum / maxPts; // Calculate current probability

            // Accumulate result for points >= k since we need to stay in this range
            if (i < k) {
                windowSum += dp[i];
            } else {
                result += dp[i];
            }

            // Maintain the window size (maxPts): slide the window
            if (i - maxPts >= 0) {
                windowSum -= dp[i - maxPts];
            }
        }

        return result;
    }
};
```

The above approach effectively uses dynamic programming to calculate the probabilities for each point, maintaining a sliding window of size `maxPts` to add new probabilities and to manage accumulated sums efficiently.

