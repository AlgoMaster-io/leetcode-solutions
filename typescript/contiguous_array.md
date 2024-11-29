# [525. Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
function findMaxLength(nums: number[]): number {
    let maxLength = 0;

    // Check all possible subarrays
    for (let i = 0; i < nums.length; i++) {
        let count = 0;

        for (let j = i; j < nums.length; j++) {
            // Increment or decrement count based on value
            count += nums[j] === 1 ? 1 : -1;

            // If count is zero, it means equal number of 0s and 1s
            if (count === 0) {
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }
    }

    return maxLength;
}
```

## Approach 2: HashMap (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function findMaxLength(nums: number[]): number {
    const map = new Map<number, number>();
    map.set(0, -1); // Initialize with a count of 0 at index -1
    let maxLength = 0;
    let count = 0;

    for (let i = 0; i < nums.length; i++) {
        // Increment or decrement count based on value
        count += nums[i] === 1 ? 1 : -1;

        if (map.has(count)) {
            // Calculate the length of the subarray
            maxLength = Math.max(maxLength, i - map.get(count)!);
        } else {
            // Store the first occurrence of this count
            map.set(count, i);
        }
    }

    return maxLength;
}
```

