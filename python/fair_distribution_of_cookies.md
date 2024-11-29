# 2305. [Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

## Approach 1: Brute Force - Generate All Distributions

### Solution
```python
# Time Complexity: O(k^n)
# Space Complexity: O(k)
class Solution:
    def distributeCookies(self, cookies, k):
        return self.helper(cookies, [0] * k, 0)

    def helper(self, cookies, distribution, index):
        if index == len(cookies):
            return max(distribution)

        min_unfairness = float('inf')
        for i in range(len(distribution)):
            distribution[i] += cookies[index]
            min_unfairness = min(min_unfairness, self.helper(cookies, distribution, index + 1))
            distribution[i] -= cookies[index]

        return min_unfairness
```

## Approach 2: Backtracking with Pruning

### Solution
```python
# Time Complexity: O(k * 2^n)
# Space Complexity: O(k)
class Solution:
    def distributeCookies(self, cookies, k):
        distribution = [0] * k
        return self.helper(cookies, distribution, 0, 0, float('inf'))

    def helper(self, cookies, distribution, index, current_max, min_unfairness):
        if index == len(cookies):
            return min(min_unfairness, current_max)

        for i in range(len(distribution)):
            distribution[i] += cookies[index]
            min_unfairness = self.helper(cookies, distribution, index + 1, max(current_max, distribution[i]), min_unfairness)
            distribution[i] -= cookies[index]

            if distribution[i] == 0:  # Optimizing to prevent unnecessary states
                break

        return min_unfairness
```

## Approach 3: Dynamic Programming with Bitmask

### Solution
```python
# Time Complexity: O(n*2^n)
# Space Complexity: O(2^n)
class Solution:
    def distributeCookies(self, cookies, k):
        n = len(cookies)
        sum_mask = [0] * (1 << n)
        
        # Precompute the sum of each subset
        for i in range(1 << n):
            for j in range(n):
                if (i & (1 << j)) != 0:
                    sum_mask[i] += cookies[j]

        # dp[mask] is the number of k subsets with the j-th subset (sum of mask is taken) having the minimum maximum sum
        dp = [[float('inf')] * (1 << n) for _ in range(k + 1)]
        dp[0][0] = 0

        for i in range(1, k + 1):
            for mask in range(1 << n):
                submask = mask
                while submask > 0:
                    dp[i][mask] = min(dp[i][mask], max(dp[i - 1][mask ^ submask], sum_mask[submask]))
                    submask = (submask - 1) & mask
        
        return dp[k][(1 << n) - 1]
```

