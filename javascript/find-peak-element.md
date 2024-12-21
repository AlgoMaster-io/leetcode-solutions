# [Leetcode 162: Find Peak Element](https://leetcode.com/problems/find-peak-element/)

## Approaches
1. [Linear Scan](#linear-scan)
2. [Iterative Binary Search](#iterative-binary-search)
3. [Recursive Binary Search](#recursive-binary-search)

---

## Linear Scan

### Intuition
The simplest approach is to sequentially scan through the array to identify any peak element — that is, an element that is greater than its neighbors. This method leverages the definition of a peak and stops at the first instance found.

### Algorithm
1. Traverse through the array using a loop.
2. For each element, check if it is greater than its neighbors.
3. Return the index of the first element meeting the peak criteria.
4. Edge cases: Consider scenarios where the peak could be at either end of the array or the array is of minimal size.

### Code Implementation
```javascript
const findPeakElement = function(nums) {
    for (let i = 0; i < nums.length; i++) {
        // Check if the current element is greater than both its neighbors
        if ((i === 0 || nums[i] > nums[i - 1]) && 
            (i === nums.length - 1 || nums[i] > nums[i + 1])) {
            return i;
        }
    }
    return -1; // Should never reach here if input is valid
};
```

### Complexity
- **Time Complexity:** O(n), where n is the number of elements in the array. We might need to check every element in the worst case.
- **Space Complexity:** O(1), as we are not using any extra space besides a few variables.

---

## Iterative Binary Search

### Intuition
A more efficient approach is to use binary search, leveraging the properties of peaks in the array. By dividing the array and examining if the mid element forms a peak, we can decisively move towards a region containing at least one peak.

### Algorithm
1. Initialize search space with `left` at 0 and `right` at the last index.
2. While `left` is less than `right`, find the mid-point.
3. Compare the `nums[mid]` with `nums[mid + 1]`.
   - If `nums[mid] > nums[mid + 1]`, there might be a peak on the left, so move `right` to `mid`.
   - Otherwise, move `left` to `mid + 1`.
4. When `left` equals `right`, that index is the peak.

### Code Implementation
```javascript
const findPeakElement = function(nums) {
    let left = 0, right = nums.length - 1;
    while (left < right) {
        let mid = Math.floor((left + right) / 2);
        // Compare mid with its next element
        if (nums[mid] > nums[mid + 1]) {
            right = mid; // Potential peak is in the left half
        } else {
            left = mid + 1; // Peak must be in the right half
        }
    }
    return left;
};
```

### Complexity
- **Time Complexity:** O(log n), where n is the number of elements. Binary search reduces the search space by half with each step.
- **Space Complexity:** O(1), since no extra space is used beyond a few pointers/variables.

---

## Recursive Binary Search

### Intuition
This approach uses recursion for binary search, maintaining the same principles as the iterative version. By recursively refining the search space, it locates the peak similarly.

### Algorithm
1. Define a recursive function that takes the current bounds, `left` and `right`.
2. If `left` equals `right`, return `left`.
3. Otherwise, calculate `mid` as the midpoint.
4. Compare the elements at `mid` and `mid + 1`.
5. Recursively search the half of the array where the peak might exist.
   
### Code Implementation
```javascript
const findPeakElement = function(nums) {
    const search = (left, right) => {
        if (left === right) return left;
        let mid = Math.floor((left + right) / 2);
        // Determine which side to explore based on comparison of mid & mid+1
        if (nums[mid] > nums[mid + 1]) {
            return search(left, mid); // Search in the left half
        } else {
            return search(mid + 1, right); // Search in the right half
        }
    };
    return search(0, nums.length - 1);
};
```

### Complexity
- **Time Complexity:** O(log n), due to the nature of binary search.
- **Space Complexity:** O(log n), due to the recursive call stack used for maintaining the function calls.

