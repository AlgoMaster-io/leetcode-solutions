# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
function minSubArrayLen(target: number, nums: number[]): number {
    let minLength = Number.MAX_SAFE_INTEGER;

    // Iterate over all possible starting points
    for (let i = 0; i < nums.length; i++) {
        let sum = 0;

        // Iterate over all possible ending points
        for (let j = i; j < nums.length; j++) {
            sum += nums[j];

            // Check if the sum is greater than or equal to the target
            if (sum >= target) {
                minLength = Math.min(minLength, j - i + 1);
                break; // Break as the subarray length cannot decrease further
            }
        }
    }

    return minLength === Number.MAX_SAFE_INTEGER ? 0 : minLength;
}
```

## Approach 2: Sliding Window (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
function minSubArrayLen(target: number, nums: number[]): number {
    let left = 0;
    let sum = 0;
    let minLength = Number.MAX_SAFE_INTEGER;

    // Sliding window
    for (let right = 0; right < nums.length; right++) {
        sum += nums[right];

        // Shrink the window until the sum is less than the target
        while (sum >= target) {
            minLength = Math.min(minLength, right - left + 1);
            sum -= nums[left];
            left++;
        }
    }

    return minLength === Number.MAX_SAFE_INTEGER ? 0 : minLength;
}
```

