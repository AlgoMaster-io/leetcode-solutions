# [Leetcode 15: 3Sum](https://leetcode.com/problems/3sum/)

## Solutions
- [Brute Force Approach](#brute-force-approach)
- [Two Pointers Approach](#two-pointers-approach)

### Brute Force Approach
The brute force approach involves using three nested loops to check every possible triplet in the array, identifying triplets that sum to zero. Although intuitive, this method is computationally expensive and not feasible for larger inputs.

**Intuition:**
1. Iterate over the array with three nested loops to consider every possible triplet.
2. For each triplet, check if the sum is equal to zero.
3. Collect all unique triplets.

```typescript
function threeSumBruteForce(nums: number[]): number[][] {
    const result: number[][] = [];
    const length = nums.length;
    
    // Iterate over each element in array using three loops to find triplets
    for (let i = 0; i < length - 2; i++) {
        for (let j = i + 1; j < length - 1; j++) {
            for (let k = j + 1; k < length; k++) {
                // Check if the current triplet sums to zero
                if (nums[i] + nums[j] + nums[k] === 0) {
                    const triplet = [nums[i], nums[j], nums[k]].sort((a, b) => a - b);
                    // Avoid adding duplicate triplets by checking if triplet is not already in the result
                    if (!result.some(r => r[0] === triplet[0] && r[1] === triplet[1] && r[2] === triplet[2])) {
                        result.push(triplet);
                    }
                }
            }
        }
    }
    return result;
}
```

- **Time Complexity:** O(n^3), where n is the number of elements in the array.
- **Space Complexity:** O(m), where m is the number of unique triplets.

### Two Pointers Approach
A much more efficient method involves sorting the array and using a two-pointer technique to find the triplets. This reduces the time complexity drastically compared to the brute force method.

**Intuition:**
1. Sort the array.
2. Fix the first element of the triplet and use a two-pointer approach to find the other two elements.
3. Move the left pointer one step at a time unless the sum is greater, where you move the right pointer backward.
4. Continue this until the pointers meet, and skip duplicate elements to ensure unique triplets.

```typescript
function threeSum(nums: number[]): number[][] {
    const result: number[][] = [];
    nums.sort((a, b) => a - b);  // Sorting the array
    
    for (let i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue; // Skip duplicates
        
        let left = i + 1, right = nums.length - 1;
        
        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];
            
            if (sum === 0) {
                result.push([nums[i], nums[left], nums[right]]);
                
                // Avoid duplicates by skipping same elements
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;
                
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
}
```

- **Time Complexity:** O(n^2), after sorting the array, where n is the number of elements.
- **Space Complexity:** O(m), for storing the results, where m is the number of unique triplets found. The sorting step may require O(n) space depending on the sorting algorithm used internally. (Typically O(log n) for Timsort).

Both approaches highlight different levels of efficiency in solving the same problem. The two-pointer technique is preferred for its optimal performance, especially on larger datasets.

