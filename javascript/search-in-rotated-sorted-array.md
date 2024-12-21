# [Leetcode 33: Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Approaches:
- [Approach 1: Linear Search](#approach-1-linear-search)
- [Approach 2: Modified Binary Search](#approach-2-modified-binary-search)

---

### Approach 1: Linear Search

#### Intuition:
A straightforward approach to find an element in an array is to perform a linear search. We iterate through each element to check if it matches the target value. This approach doesn't leverage the sorted nature of the parts of the array.

#### Code:
```javascript
function search(nums, target) {
    // Iterate over each element in the array
    for (let i = 0; i < nums.length; i++) {
        // Check if the current element is the target
        if (nums[i] === target) {
            return i; // Return the index if found
        }
    }
    return -1; // Return -1 if the target is not found
}
```

#### Time Complexity:
- **O(n)**: We might have to check each element in the array.

#### Space Complexity:
- **O(1)**: No extra space other than input and variables.

---

### Approach 2: Modified Binary Search

#### Intuition:
The problem describes a rotated sorted array which can effectively be dealt with using a binary search algorithm. The rotated array has two sorted sub-arrays. By finding a pivot, we can decide which part of the array to continue searching in.

1. Calculate the middle index.
2. Check if the middle element is the target.
3. Determine whether the left half or the right half is sorted.
4. Decide which half to continue searching in:
   - If the left half is sorted:
     - Check if the target is within the left half; if yes, continue your search there as usual binary search.
     - Otherwise, move to the right half.
   - If the right half is sorted:
     - Check if the target is within the right half; if yes, continue your search there as usual binary search.
     - Otherwise, move to the left half.

#### Code:
```javascript
function search(nums, target) {
    let left = 0, right = nums.length - 1;
    
    // Perform binary search
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);

        // Check if the mid element is the target
        if (nums[mid] === target) {
            return mid;
        }

        // Determine which half is sorted
        if (nums[left] <= nums[mid]) {
            // Left half is sorted
            if (nums[left] <= target && target < nums[mid]) {
                // Target is in the left half
                right = mid - 1;
            } else {
                // Target must be in the right half
                left = mid + 1;
            }
        } else {
            // Right half is sorted
            if (nums[mid] < target && target <= nums[right]) {
                // Target is in the right half
                left = mid + 1;
            } else {
                // Target must be in the left half
                right = mid - 1;
            }
        }
    }
    return -1; // Target not found in the array
}
```

#### Time Complexity:
- **O(log n)**: By leveraging the characteristics of the rotated sorted array, each decision cuts the problem size in half, typical of binary search.

#### Space Complexity:
- **O(1)**: No additional space is used other than the variables for indices.

By employing a modified binary search, we use the properties of the rotated array to maintain efficiency while identifying the target value. This approach is optimal and leverages binary search affordances even in a rotated context.

