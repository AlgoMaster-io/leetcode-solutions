# [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approach 1: Dynamic Programming with Sliding Window (Deque)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)

function maxResult(nums, k) {
    const deque = []; // Stores indices of potential max scores
    const dp = new Array(nums.length).fill(0);
    dp[0] = nums[0];
    deque.push(0);

    for (let i = 1; i < nums.length; i++) {
        // Remove indices that are out of the current window
        if (deque.length && deque[0] < i - k) {
            deque.shift();
        }

        // Update dp[i] using the max value in the deque
        dp[i] = nums[i] + dp[deque[0]];

        // Maintain the deque to keep indices in decreasing order of their dp values
        while (deque.length && dp[deque[deque.length - 1]] <= dp[i]) {
            deque.pop();
        }

        deque.push(i);
    }

    return dp[nums.length - 1];
}
```

## Approach 2: Priority Queue (Heap-Based)

### Solution
```javascript
// Time Complexity: O(n log k)
// Space Complexity: O(k)

const maxResult = function(nums, k) {
    const maxHeap = []; // Max-heap using an array
    maxHeap.push([nums[0], 0]); // Store [score, index]
    let score = nums[0];

    for (let i = 1; i < nums.length; i++) {
        // Remove elements outside the current window
        while (maxHeap[0][1] < i - k) {
            maxHeap.shift();
        }

        // The current score is the sum of the max score in the heap and nums[i]
        score = nums[i] + maxHeap[0][0];
        maxHeap.push([score, i]);

        // Keep the heap sorted (manual sorting to handle max-heap property in JavaScript)
        maxHeap.sort((a, b) => b[0] - a[0]);
    }

    return score;
}
```

