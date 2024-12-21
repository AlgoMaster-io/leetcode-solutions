# [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

## Approaches
1. [Linear Search](#linear-search)
2. [Binary Search](#binary-search)

## Linear Search

### Intuition
When thinking about finding the minimum value in a rotated sorted array, a brute-force approach involves simply traversing the entire array and keeping track of the smallest element encountered. This is because a linear scan would allow us to identify the minimum element without any additional logic, albeit not efficiently.

### Approach
- Traverse the array from the first to the last element.
- Maintain a variable to store the current minimum element, initially set to the first element of the array.
- For each element in the array, update the current minimum if the current element is less than the existing minimum.
- By the end of the loop, the stored minimum will be the smallest element in the array.

```java
public class Solution {
    public int findMin(int[] nums) {
        // Initialize the minimum number with the first element
        int min = nums[0];
        
        // Traverse the entire array
        for (int i = 1; i < nums.length; i++) {
            // If a smaller element is found, update the minimum
            if (nums[i] < min) {
                min = nums[i];
            }
        }
        
        // Return the found minimum
        return min;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of elements in the array. In the worst case, we need to scan each element once.
- **Space Complexity**: O(1). We use only a constant amount of additional space.

## Binary Search

### Intuition
The rotated sorted array still retains some properties of a sorted array. In a non-rotated sorted array, the first element is the smallest. Upon rotation, this property is only minimally affected. Hence, a more efficient approach can leverage binary search, exploiting the array's semi-sorted nature.

### Approach
- Use two pointers, `left` and `right`, initialized to the start and end of the array respectively.
- While `left` is less than `right`, calculate the middle index.
- If the middle element is greater than the element at `right`, it means the minimum is to the right of `mid`.
- If the middle element is less than or equal to the element at `right`, it means the minimum is at `mid` or to the left of `mid`.
- Adjust the `left` or `right` pointers accordingly.
- When the loop exits, `left` will point to the smallest element.

```java
public class Solution {
    public int findMin(int[] nums) {
        // Initialize the left and right pointers
        int left = 0, right = nums.length - 1;
        
        // While there's a range to search
        while (left < right) {
            // Calculate the midpoint
            int mid = left + (right - left) / 2;
            
            // If middle element is greater than the right, min is in the right half
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                // min must be in the left half (including mid)
                right = mid;
            }
        }
        
        // The minimum element is at the pointer `left`
        return nums[left];
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(log n), where n is the number of elements in the array. Binary search reduces the search space by half each time.
- **Space Complexity**: O(1). Similarly, only a constant amount of space is used.

Using the binary search approach leverages the rotated, yet sorted property of the array, efficiently finding the minimum element with logarithmic time complexity, making it far more optimal than a linear search.

