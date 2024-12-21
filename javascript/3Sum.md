# [Leetcode 15: 3Sum](https://leetcode.com/problems/3sum/)

### Approaches
- [Brute Force](#brute-force)
- [Two Pointers](#two-pointers)

## Brute Force

For the 3Sum problem, the brute force approach would involve checking every possible triplet in the array to see if they sum to zero. This can be achieved using three nested loops. While straightforward to implement, this method is not optimal given its high time complexity.

### Detailed Intuition

1. **Triplets Check**: Iterate through each possible combination of three numbers in the array.
2. **Sum Check**: For each combination, compute the sum of the triplet.
3. **Zero Sum**: If the sum of the triplet is zero, add this triplet to the result list.
4. **Avoid Duplicates**: As we're working with sets of numbers, ensure no duplicates are added to the result list by checking if the triplet already exists in the result list.

### Code
```javascript
var threeSum = function(nums) {
    let result = [];
    let n = nums.length;

    // Sort the array to help avoid duplicate triplets later
    nums.sort((a, b) => a - b);

    // Iterate for each possible triplet
    for (let i = 0; i < n - 2; i++) {
        for (let j = i + 1; j < n - 1; j++) {
            for (let k = j + 1; k < n; k++) {
                if (nums[i] + nums[j] + nums[k] === 0) {
                    let triplet = [nums[i], nums[j], nums[k]];
                    if (!result.some(r => r[0] === triplet[0] && r[1] === triplet[1] && r[2] === triplet[2])) {
                        result.push(triplet);
                    }
                }
            }
        }
    }

    return result;
};
```

### Time and Space Complexity
- **Time Complexity**: \(O(n^3)\) as three nested loops are used.
- **Space Complexity**: \(O(n)\) for storing the results (though in the worst case, every triplet might sum to zero).

## Two Pointers

This approach leverages sorting in conjunction with the two-pointer technique to optimize the sum checks, reducing the overall time complexity.

### Detailed Intuition

1. **Sorting**: First, sort the array. This will make it easier to avoid duplicates and use the two-pointer technique.
2. **Iterate with the first number**: Loop through the array with one number fixed.
3. **Two-Pointer Technique**:
   - Use two pointers, one starting just after the fixed number and the other at the end of the array.
   - Calculate the sum of the fixed number and the two pointers.
   - Based on the sum, adjust the pointers:
     - If the sum is zero, record the triplet, then skip duplicates by checking adjacent values for the pointers.
     - If the sum is less than zero, move the left pointer to the right to increase the sum.
     - If the sum is more than zero, move the right pointer to the left to decrease the sum.

### Code
```javascript
var threeSum = function(nums) {
    let result = [];
    let n = nums.length;

    // Sort the array
    nums.sort((a, b) => a - b);

    for (let i = 0; i < n-2; i++) {
        // Avoid duplicate numbers for the fixed number
        if (i > 0 && nums[i] === nums[i-1]) continue;

        let left = i + 1;
        let right = n - 1;

        while (left < right) {
            let sum = nums[i] + nums[left] + nums[right];

            if (sum === 0) {
                result.push([nums[i], nums[left], nums[right]]);
                // Avoid duplicate numbers
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;
                // Move pointers inward
                left++;
                right--;
            } else if (sum < 0) {
                left++;
            } else {
                right--;
            }
        }
    }

    return result;
};
```

### Time and Space Complexity
- **Time Complexity**: \(O(n^2)\), reduced from \(O(n^3)\) because we use two pointers instead of a third loop.
- **Space Complexity**: \(O(n)\) for storing the results, though the sorting process may take additional space.

