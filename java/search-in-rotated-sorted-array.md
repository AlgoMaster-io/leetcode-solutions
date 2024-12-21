# [Leetcode 33: Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Approaches

1. [Linear Search](#linear-search)
2. [Binary Search - Find Pivot, then Search](#binary-search-find-pivot-then-search)
3. [Optimized Binary Search](#optimized-binary-search)

---

### Linear Search

#### Intuition

The simplest approach to solve this problem is to iterate through the array and check each element if it matches the target. This brute force approach is straightforward but not efficient.

#### Code

```java
public int searchLinear(int[] nums, int target) {
    // Iterate through each element in the array.
    for (int i = 0; i < nums.length; i++) {
        // If the current element is the target, return its index.
        if (nums[i] == target) {
            return i;
        }
    }
    // If the target is not found, return -1.
    return -1;
}
```

#### Complexity Analysis

- **Time Complexity**: O(n), where n is the number of elements in the array. This is because each element is checked linearly.
- **Space Complexity**: O(1), no additional space is used.

---

### Binary Search - Find Pivot, then Search

#### Intuition

The array is rotated at some pivot, and this affects the normal binary search. The idea is to first find this pivot point, then determine in which of the two sub-arrays (from 0 to pivot, or pivot to n-1) the target resides, and perform a binary search in the appropriate sub-array.

#### Code

```java
public int searchPivotBinary(int[] nums, int target) {
    if (nums.length == 0) return -1;

    int left = 0, right = nums.length - 1;
    
    // Find the pivot point where the array was rotated
    while (left < right) {
        int mid = left + (right - left) / 2;
        // If mid element is greater than the rightmost element, pivot is in the right half
        if (nums[mid] > nums[right]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    // 'left' is the pivot
    int pivot = left;
    left = 0;
    right = nums.length - 1;
    
    // Determine which side of the pivot the target lies
    if (target >= nums[pivot] && target <= nums[right]) {
        left = pivot;
    } else {
        right = pivot - 1;
    }
    
    // Standard binary search
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    // If we reach here, target is not in array
    return -1;
}
```

#### Complexity Analysis

- **Time Complexity**: O(log n), finding the pivot and searching in the sub-array both take logarithmic time.
- **Space Complexity**: O(1), no additional space is used.

---

### Optimized Binary Search

#### Intuition

Since the array is sorted and only rotated, a single pass binary search can be designed. By checking the sorted property, we can decide which half to continue the search on. This eliminates the need for a separate pivot finding step, thereby optimizing the binary search process.

#### Code

```java
public int searchOptimizedBinary(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        // Check if mid element is the target
        if (nums[mid] == target) {
            return mid;
        }
        
        // Determine which side is sorted
        if (nums[left] <= nums[mid]) { // Left part is sorted
            if (nums[left] <= target && target < nums[mid]) {
                // Target is within the sorted left part
                right = mid - 1;
            } else {
                // Target is in the right part
                left = mid + 1;
            }
        } else { // Right part is sorted
            if (nums[mid] < target && target <= nums[right]) {
                // Target is within the sorted right part
                left = mid + 1;
            } else {
                // Target is in the left part
                right = mid - 1;
            }
        }
    }
    
    // If we reach here, target is not in array
    return -1;
}
```

#### Complexity Analysis

- **Time Complexity**: O(log n), executing binary search directly.
- **Space Complexity**: O(1), no additional space is used.

Each of these approaches provides a different strategy for tackling the problem, with the optimized binary search being the most efficient due to its direct handling of the rotated array.

