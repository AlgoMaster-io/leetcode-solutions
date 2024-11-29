# [523. Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
    checkSubarraySum(nums: number[], k: number): boolean {
        const n: number = nums.length;

        // Check every possible subarray
        for (let i = 0; i < n - 1; i++) {
            let sum: number = nums[i];
            for (let j = i + 1; j < n; j++) {
                sum += nums[j];
                if (k === 0 ? sum === 0 : sum % k === 0) {
                    return true; // Subarray sum is a multiple of k
                }
            }
        }

        return false; // No valid subarray found
    }
}
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    checkSubarraySum(nums: number[], k: number): boolean {
        const remainderMap = new Map<number, number>();
        remainderMap.set(0, -1); // Initialize with a remainder of 0 at index -1
        let sum: number = 0;

        for (let i = 0; i < nums.length; i++) {
            sum += nums[i];

            // Compute the remainder when divided by k
            const remainder: number = k === 0 ? sum : sum % k;

            // If the remainder already exists
            if (remainderMap.has(remainder)) {
                // Check if the subarray length is at least 2
                if (i - (remainderMap.get(remainder) ?? 0) > 1) {
                    return true;
                }
            } else {
                // Store the index of this remainder
                remainderMap.set(remainder, i);
            }
        }

        return false; // No valid subarray found
    }
}
```

