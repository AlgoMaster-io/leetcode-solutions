# [Leetcode Problem 35: Search Insert Position](https://leetcode.com/problems/search-insert-position/)

## Approaches
1. [Linear Search](#solution-1-linear-search)
2. [Binary Search](#solution-2-binary-search)

---

## **Solution 1: Linear Search**

### Intuition
The simplest approach to tackle this problem is to perform a linear search. We iterate through the array and find the position where the target should be inserted. This approach is straightforward as we compare the target with each element in the array one by one. 

### Code
```javascript
function searchInsert(nums, target) {
    // Iterate over each element in the array
    for (let i = 0; i < nums.length; i++) {
        // If current element is greater or equal to the target,
        // return the current index as the insertion position
        if (nums[i] >= target) {
            return i;
        }
    }
    // If target is greater than all elements in the array,
    // Insert at the end so return the length of the array
    return nums.length;
}
```

### Complexity Analysis
- **Time Complexity**: O(n) - In the worst case, we may have to iterate through all elements if the target is larger than all other elements.
- **Space Complexity**: O(1) - We are using a constant amount of extra space.

---

## **Solution 2: Binary Search**

### Intuition
To improve on the linear search, we can use binary search to find the insert position efficiently. This approach leverages the fact that the array is sorted, allowing us to eliminate half of the elements after each comparison. 

### Code
```javascript
function searchInsert(nums, target) {
    // Initialize two pointers, left and right, for binary search
    let left = 0;
    let right = nums.length - 1;
    
    // While there is a valid range for searching
    while (left <= right) {
        // Calculate the mid point of the current range
        const mid = Math.floor((left + right) / 2);
        
        if (nums[mid] === target) {
            // Target is found, return the mid index
            return mid;
        } else if (nums[mid] < target) {
            // Target is greater, ignore left half
            left = mid + 1;
        } else {
            // Target is smaller, ignore right half
            right = mid - 1;
        }
    }
    
    // Return the insertion point which is the left index
    return left;
}
```

### Complexity Analysis
- **Time Complexity**: O(log n) - Binary search reduces the search space by half with each iteration.
- **Space Complexity**: O(1) - We only use a few extra variables, regardless of the input size, thus making the space usage constant.

By using binary search, we significantly improve the efficiency of our solution and make it suitable for large arrays.

