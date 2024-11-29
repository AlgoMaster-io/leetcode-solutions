# [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

## Approach 1: Brute Force

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
function moveZeroes(nums) {
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] === 0) {
            for (let j = i + 1; j < nums.length; j++) {
                if (nums[j] !== 0) {
                    // Swap the elements
                    let temp = nums[i];
                    nums[i] = nums[j];
                    nums[j] = temp;
                    break;
                }
            }
        }
    }
}
```

## Approach 2: Two-Pointer Technique

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function moveZeroes(nums) {
    let lastNonZeroFoundAt = 0; // Index to place the next non-zero element

    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== 0) {
            nums[lastNonZeroFoundAt++] = nums[i];
        }
    }

    // Fill the remaining positions with zeros
    for (let i = lastNonZeroFoundAt; i < nums.length; i++) {
        nums[i] = 0;
    }
}
```

## Approach 3: Optimal Two-Pointer with Swapping

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function moveZeroes(nums) {
    let left = 0; // Pointer to track the position of the next non-zero element

    for (let right = 0; right < nums.length; right++) {
        if (nums[right] !== 0) {
            // Swap the non-zero element with the left pointer
            let temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;
            left++; // Increment the left pointer
        }
    }
}
```

