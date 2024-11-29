# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
    public subarraysDivByK(nums: number[], k: number): number {
        let count = 0;

        // Check all subarrays
        for (let i = 0; i < nums.length; i++) {
            let sum = 0;

            for (let j = i; j < nums.length; j++) {
                sum += nums[j];

                // Check if the sum is divisible by k
                if (sum % k === 0) {
                    count++;
                }
            }
        }

        return count;
    }
}
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(k)
class Solution {
    public subarraysDivByK(nums: number[], k: number): number {
        const remainderMap = new Map<number, number>();
        remainderMap.set(0, 1); // Initialize with a remainder of 0

        let count = 0;
        let sum = 0;

        for (const num of nums) {
            sum += num;
            let remainder = sum % k;

            // Normalize the remainder to always be non-negative
            if (remainder < 0) {
                remainder += k;
            }

            // If the remainder exists in the map, add its frequency to the count
            if (remainderMap.has(remainder)) {
                count += remainderMap.get(remainder)!;
            }

            // Update the frequency of the current remainder
            remainderMap.set(remainder, (remainderMap.get(remainder) || 0) + 1);
        }

        return count;
    }
}
```

