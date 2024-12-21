# [Leetcode 153: Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

## Table of Contents
1. [Approach 1: Linear Scan](#approach-1-linear-scan)
2. [Approach 2: Binary Search (Optimal)](#approach-2-binary-search-optimal)

## Approach 1: Linear Scan

### Intuition
The simplest and most straightforward method for finding the minimum in a rotated sorted array is to perform a linear scan of the array, comparing each element to the current minimum. The smallest element found during our scan will be the answer.

### Implementation

```csharp
public class Solution {
    public int FindMin(int[] nums) {
        // Assume the first element is the smallest
        int min = nums[0];
        
        // Iterate through the array
        for(int i = 1; i < nums.Length; i++) {
            // If a smaller element is found, update min
            if(nums[i] < min) {
                min = nums[i];
            }
        }
        
        // Return the smallest element found
        return min;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of elements in the array. This is because we traverse the entire array once.
- **Space Complexity**: O(1), as no additional space is required beyond a few integer variables.

## Approach 2: Binary Search (Optimal)

### Intuition
One of the efficient ways to solve this problem is by using binary search. Since the array is rotated, it consists of two sorted subarrays. Our task is to find the point of rotation, which is the smallest element in the array. The key observation is that, in a rotated sorted array, any position in the array is either part of the left sorted segment or the right sorted segment. Therefore, we can utilize binary search to find this crossover point efficiently.

### Steps
1. **Identify Midpoint**: Calculate the midpoint of the current segment.
2. **Determine Search Space**:
   - Check if the middle element is greater than the rightmost element. If it is, it means the minimum value lies in the right half of the array.
   - Otherwise, the minimum value lies in the left half or could be the middle element itself.
3. **Narrow Down Search Space**: Adjust the search bounds based on the above conditions.
4. **Loop Until Convergence**: Continue this process until the search space consists of a single element, which will be the minimum value.

### Implementation

```csharp
public class Solution {
    public int FindMin(int[] nums) {
        int left = 0;
        int right = nums.Length - 1;
        
        // Perform the binary search
        while (left < right) {
            int mid = left + (right - left) / 2;

            // Compare mid element with the rightmost element
            if (nums[mid] > nums[right]) {
                // Minimum is in the right part
                left = mid + 1;
            } else {
                // Minimum is in the left part including mid
                right = mid;
            }
        }
        
        // left and right converge to the minimum element
        return nums[left];
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(log n), where n is the number of elements in the array. The log factor comes from binary search which halves the search space every iteration.
- **Space Complexity**: O(1), as we utilize constant additional space regardless of the input size.

By employing the binary search technique, we efficiently reduce the time complexity to logarithmic time, making it the optimal solution for this problem.

