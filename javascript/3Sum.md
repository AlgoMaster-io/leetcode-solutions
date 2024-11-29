# [15. 3Sum](https://leetcode.com/problems/3sum/)

## Approach 1: Brute Force (Basic)

### Solution
```javascript
// Time Complexity: O(n^3)
// Space Complexity: O(1) (excluding output list)
function threeSum(nums) {
    const result = new Set();
    const n = nums.length;

    // Iterate through all possible triplets
    for (let i = 0; i < n - 2; i++) {
        for (let j = i + 1; j < n - 1; j++) {
            for (let k = j + 1; k < n; k++) {
                if (nums[i] + nums[j] + nums[k] === 0) {
                    const triplet = [nums[i], nums[j], nums[k]].sort((a, b) => a - b);
                    result.add(triplet.toString());  // Add triplet as a string to the set to avoid duplicates
                }
            }
        }
    }

    // Convert set back to array of arrays
    return Array.from(result).map(triplet => triplet.split(',').map(Number));
}
```

## Approach 2: Two Pointers (Optimal)

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n) (for sorting or output list)
function threeSum(nums) {
    const result = [];
    nums.sort((a, b) => a - b);  // Sort the array to use two-pointer technique

    for (let i = 0; i < nums.length - 2; i++) {
        // Skip duplicates for the first element
        if (i > 0 && nums[i] === nums[i - 1]) {
            continue;
        }

        let left = i + 1;
        let right = nums.length - 1;

        // Two-pointer approach
        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];
            if (sum === 0) {
                result.push([nums[i], nums[left], nums[right]]);

                // Skip duplicates for the second and third elements
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;

                left++;
                right--;
            } else if (sum < 0) {
                left++;  // Increase the sum
            } else {
                right--;  // Decrease the sum
            }
        }
    }

    return result;
}
```

