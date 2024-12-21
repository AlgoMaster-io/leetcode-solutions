# [Leetcode 1696: Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approaches
1. [Dynamic Programming with Array Iteration](#dynamic-programming-with-array-iteration)
2. [Dynamic Programming with Monotonic Deque](#dynamic-programming-with-monotonic-deque)

## 1. Dynamic Programming with Array Iteration

### Intuition
The problem essentially asks us to determine the maximum score we can achieve as we jump across the array with a fixed maximum step size. The idea is to use dynamic programming to keep track of the maximum score we can obtain to the current position.

We will iterate over the array, and for each position, we'll calculate the maximum possible score from the previous `k` positions. This approach will efficiently solve the problem, producing correct results.

### Implementation
```typescript
function maxResult(nums: number[], k: number): number {
    const dp: number[] = Array(nums.length).fill(Number.NEGATIVE_INFINITY);
    dp[0] = nums[0];

    // Iterate over the array
    for (let i = 1; i < nums.length; i++) {
        // Calculate maximum score from the previous k positions
        for (let j = Math.max(0, i - k); j < i; j++) {
            dp[i] = Math.max(dp[i], dp[j] + nums[i]);
        }
    }

    return dp[nums.length - 1];
}
```

### Time Complexity
- O(n*k), where `n` is the length of the input array, and `k` is the maximum jump length. This is because for each position, we check up to `k` previous positions.

### Space Complexity
- O(n) for storing the DP array.

## 2. Dynamic Programming with Monotonic Deque

### Intuition
The above approach can be optimized using a monotonic deque, which helps keep track of the maximum possible scores in a sliding window fashion. This helps reduce the redundant calculations in the previous approach. The goal is to maintain the indices of elements in the deque in such a way that the front always contains the index of the maximum score in the current window.

### Implementation
```typescript
function maxResult(nums: number[], k: number): number {
    const deque: number[] = [];
    const dp: number[] = Array(nums.length).fill(0);
    dp[0] = nums[0];
    deque.push(0);

    for (let i = 1; i < nums.length; i++) {
        // Maintain the deque to have the maximum at the front
        if (deque[0] < i - k) {
            deque.shift();
        }

        // Add maximum score from the deque
        dp[i] = dp[deque[0]] + nums[i];

        // Maintain deque properties
        while (deque.length > 0 && dp[i] >= dp[deque[deque.length - 1]]) {
            deque.pop();
        }

        // Add current index to deque
        deque.push(i);
    }

    return dp[nums.length - 1];
}
```

### Time Complexity
- O(n), as each element is added and removed from the deque at most once.

### Space Complexity
- O(n) for storing the DP array and potentially additional space for deque operations, although this tends to be much less than `n` in practice.

