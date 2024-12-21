# [Leetcode 34: Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Approaches 
- [Approach 1: Linear Search](#approach-1-linear-search)
- [Approach 2: Binary Search](#approach-2-binary-search)
- [Approach 3: Optimized Binary Search](#approach-3-optimized-binary-search)

## Approach 1: Linear Search

### Intuition
The simplest way to solve the problem is to perform a linear scan to find the first and last position of the target element. We iterate over the array, and whenever we encounter the target, we update the first occurrence if not already found and keep updating the last occurrence until the end.

### Code
```csharp
public int[] SearchRange(int[] nums, int target) {
    int firstPosition = -1;
    int lastPosition = -1;
    
    // Linear scan to find first and last position
    for (int i = 0; i < nums.Length; i++) {
        if (nums[i] == target) {
            if (firstPosition == -1) {
                firstPosition = i; // Set first occurrence
            }
            lastPosition = i; // Set last occurrence as it might be updated
        }
    }
    
    return new int[] {firstPosition, lastPosition};
}
```

### Complexity Analysis
- **Time Complexity**: O(n) - In the worst case, we might have to scan the entire array.
- **Space Complexity**: O(1) - Only a fixed amount of extra space is used.

## Approach 2: Binary Search

### Intuition
Since the array is sorted, we can use binary search to efficiently find the positions. This will reduce the time complexity compared to linear search. We can perform a binary search for the first occurrence and another binary search for the last occurrence of the target.

### Code
```csharp
public int[] SearchRange(int[] nums, int target) {
    return new int[] { FindFirstPosition(nums, target), FindLastPosition(nums, target) };
}

private int FindFirstPosition(int[] nums, int target) {
    int left = 0;
    int right = nums.Length - 1;
    int firstPosition = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] >= target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }

        if (nums[mid] == target) {
            firstPosition = mid;
        }
    }
    
    return firstPosition;
}

private int FindLastPosition(int[] nums, int target) {
    int left = 0;
    int right = nums.Length - 1;
    int lastPosition = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }

        if (nums[mid] == target) {
            lastPosition = mid;
        }
    }
    
    return lastPosition;
}
```

### Complexity Analysis
- **Time Complexity**: O(log n) - Binary search splits the search space into half each time.
- **Space Complexity**: O(1) - Constant space used for variables.

## Approach 3: Optimized Binary Search

### Intuition
The optimized solution uses a single binary search method with a modified condition to find both the first and last positions. The idea is to run binary search once and use conditions during the search to determine both positions.

### Code
```csharp
public int[] SearchRange(int[] nums, int target) {
    int[] result = new int[] {-1, -1};
    if (nums == null || nums.Length == 0) return result;

    int low = 0;
    int high = nums.Length - 1;
    
    // Find first occurrence
    while (low < high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }
    
    // If target not found
    if (nums[low] != target) return result;
    
    result[0] = low;
    
    // Reset high for finding last occurrence
    high = nums.Length - 1;
    
    // Find last occurrence
    while (low < high) {
        int mid = low + (high - low) / 2 + 1; // Bias the mid to the right
        if (nums[mid] > target) {
            high = mid - 1;
        } else {
            low = mid;
        }
    }
    
    result[1] = high;
    
    return result;
}
```

### Complexity Analysis
- **Time Complexity**: O(log n) - Two binary searches, each taking O(log n) time.
- **Space Complexity**: O(1) - Constant space for storing indices and other variables.

This solution provides the most efficient performance, ensuring that duplicate elements are handled correctly within their sorted positions.

