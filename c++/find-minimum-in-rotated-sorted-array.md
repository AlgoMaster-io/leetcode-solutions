# [Leetcode 153: Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

## Approaches:
1. [Linear Search Approach](#linear-search-approach)
2. [Binary Search Approach](#binary-search-approach)

---

### Linear Search Approach

The most straightforward method to find the minimum in a rotated sorted array is to use a linear search. This approach iterates through the array and simply keeps track of the smallest element found. Since the minimum element could be at the end of the array in the worst rotation, we must inspect each element.

#### Intuition:
- Iterate through the array and compare each element to the current minimum found.
- Update the minimum when a smaller element is found.

#### Time Complexity:
- **O(n)**: We check each element once.

#### Space Complexity:
- **O(1)**: We use a constant amount of extra space.

```cpp
int findMinLinear(vector<int>& nums) {
    // Initialize minimum with the first element
    int minimum = nums[0];
    
    // Traverse the entire array
    for (int i = 1; i < nums.size(); i++) {
        // Update minimum if a smaller number is found
        if (nums[i] < minimum) {
            minimum = nums[i];
        }
    }
    
    // Return the smallest number found
    return minimum;
}
```

---

### Binary Search Approach

A more efficient way to solve this problem takes advantage of the array's properties of being sorted and then rotated. Using binary search, we can find the minimum element in logarithmic time.

#### Intuition:
- Since the array is rotated, at least one part of the array is sorted.
- Use two pointers (`low`, `high`) to keep track of the subarray.
- Check the middle element. If it's greater than the high element, the minimum must be in the right part. Otherwise, it's in the left part.
- Repeat the procedure until `low` and `high` converge.

#### Detailed Steps:
1. Initialize `low` as 0 and `high` as the last index of the array.
2. While `low` is less than `high`:
   - Calculate the middle index `mid`.
   - If `nums[mid]` is greater than `nums[high]`, the minimum is in the right half, so assign `low = mid + 1`.
   - Otherwise, the minimum is in the left half or could be the middle itself, so assign `high = mid`.
3. When this loop ends, `low` should point to the smallest element.

#### Time Complexity:
- **O(log n)**: We halve the search space at each step.

#### Space Complexity:
- **O(1)**: We only use a constant amount of additional space.

```cpp
int findMinBinarySearch(vector<int>& nums) {
    // Start with the entire array range
    int low = 0, high = nums.size() - 1;
    
    // Perform binary search
    while (low < high) {
        int mid = low + (high - low) / 2;
        
        // Check if the middle element is greater than the highest element
        if (nums[mid] > nums[high]) {
            // The minimum must be in the right part
            low = mid + 1;
        } else {
            // The minimum must be in the left part or is the middle
            high = mid;
        }
    }
    
    // 'low' should point to the minimum element
    return nums[low];
}
```

Both approaches can solve the problem, but the binary search approach is optimal and preferred due to its logarithmic time complexity.

