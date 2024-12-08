# [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Approach 1: Sliding Window with Deque

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    public longestSubarray(nums: number[], limit: number): number {
        const maxDeque: number[] = []; // Stores indices of max elements in the window
        const minDeque: number[] = []; // Stores indices of min elements in the window
        let left = 0, maxLength = 0;

        for (let right = 0; right < nums.length; right++) {
            // Maintain the decreasing order in maxDeque
            while (maxDeque.length && nums[maxDeque[maxDeque.length - 1]] < nums[right]) {
                maxDeque.pop();
            }
            maxDeque.push(right);

            // Maintain the increasing order in minDeque
            while (minDeque.length && nums[minDeque[minDeque.length - 1]] > nums[right]) {
                minDeque.pop();
            }
            minDeque.push(right);

            // Check if the current window is valid
            while (nums[maxDeque[0]] - nums[minDeque[0]] > limit) {
                left++; // Shrink the window from the left
                // Remove indices out of the window
                if (maxDeque[0] < left) maxDeque.shift();
                if (minDeque[0] < left) minDeque.shift();
            }

            maxLength = Math.max(maxLength, right - left + 1); // Update the max length
        }

        return maxLength;
    }
}
```

## Approach 2: Sliding Window with TreeMap

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import TreeMap from 'red-black-tree-ts'; // Import a TreeMap equivalent library in TypeScript

class Solution {
    public longestSubarray(nums: number[], limit: number): number {
        const map = new TreeMap<number, number>(); // Stores elements and their frequencies
        let left = 0, maxLength = 0;

        for (let right = 0; right < nums.length; right++) {
            // Add the current element to the map
            map.set(nums[right], (map.get(nums[right]) ?? 0) + 1);

            // Shrink the window if the condition is violated
            while ((map.max() ?? 0) - (map.min() ?? 0) > limit) {
                map.set(nums[left], map.get(nums[left])! - 1);
                if (map.get(nums[left]) === 0) {
                    map.delete(nums[left]);
                }
                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1); // Update the max length
        }

        return maxLength;
    }
}
```

Note: The `red-black-tree-ts` is an example of a library that provides a TreeMap in TypeScript. You might need to install it or find an equivalent package if it's not installed.

