# [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

## Approach 1: Two-Pass Counting Sort

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function sortColors(nums) {
    const count = [0, 0, 0];

    // Count the occurrences of 0, 1, and 2
    for (const num of nums) {
        count[num]++;
    }

    // Overwrite the array based on the counts
    let index = 0;
    for (let i = 0; i < 3; i++) {
        while (count[i] > 0) {
            nums[index++] = i;
            count[i]--;
        }
    }
}
```

## Approach 2: One-Pass (Dutch National Flag Algorithm)

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function sortColors(nums) {
    let low = 0, mid = 0, high = nums.length - 1;

    while (mid <= high) {
        if (nums[mid] === 0) {
            // Swap nums[low] and nums[mid] and move pointers
            swap(nums, low++, mid++);
        } else if (nums[mid] === 1) {
            // Just move the mid pointer
            mid++;
        } else {
            // Swap nums[mid] and nums[high] and move high pointer
            swap(nums, mid, high--);
        }
    }
}

function swap(nums, i, j) {
    const temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```

