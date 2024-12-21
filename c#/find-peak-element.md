# [Leetcode 162: Find Peak Element](https://leetcode.com/problems/find-peak-element/)

## Navigation
- [Approach 1: Linear Scan](#approach-1-linear-scan)
- [Approach 2: Binary Search](#approach-2-binary-search)

## Approach 1: Linear Scan

### Intuition
A peak element in an array is an element that is greater than its neighbors. We can perform a simple linear scan of the array to find any such element. This approach checks each element with its next neighbor and returns the index of the first peak found.

### Code
```csharp
public int FindPeakElement(int[] nums) {
    for (int i = 0; i < nums.Length - 1; i++) {
        // Check if the current element is a peak
        if (nums[i] > nums[i + 1]) {
            return i;
        }
    }
    // If no peak was found, the last element is the peak
    return nums.Length - 1;
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of elements in the array. This is because in the worst case, we might have to scan through the entire array.
- **Space Complexity**: O(1), as no extra space is used apart from a couple of variables.

## Approach 2: Binary Search

### Intuition
The Binary Search approach leverages the property that a peak element must exist in the array. If an element is not a peak and it's greater than the element to the left, then there must be a peak somewhere right. Conversely, if it's greater than the element to the right, the peak must be somewhere left. This allows us to ignore half of the search space at each step.

### Code
```csharp
public int FindPeakElement(int[] nums) {
    int left = 0, right = nums.Length - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        // Check if mid element is greater than the next element
        if (nums[mid] > nums[mid + 1]) {
            // Potential peak found, move left
            right = mid;
        } else {
            // Peak must be to the right
            left = mid + 1;
        }
    }
    
    // Left should be at the peak element position
    return left;
}
```

### Complexity Analysis
- **Time Complexity**: O(log n), where n is the number of elements in the array. The search space is halved each step.
- **Space Complexity**: O(1), as no extra space is required beyond a few variables for indices.

