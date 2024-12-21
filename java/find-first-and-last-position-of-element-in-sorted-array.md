# [Leetcode 34: Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Approaches
1. [Linear Scan](#linear-scan)
2. [Binary Search Twice](#binary-search-twice)
3. [Optimized Binary Search](#optimized-binary-search)

### Linear Scan

#### Intuition:
The simplest way to find the first and last positions of a target element in a sorted array is to perform a linear scan. We traverse the array from the beginning to end and record the positions when the target element matches.

#### Code:
```java
public int[] searchRange(int[] nums, int target) {
    int first = -1;
    int last = -1;
    
    // Scan through the array linearly
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == target) {
            // Set the first occurrence if it's not already set
            if (first == -1) first = i;
            // Set the last occurrence to the current found index
            last = i;
        }
    }
    
    return new int[]{first, last};
}
```

#### Time Complexity:
- O(n): Because we scan through the entire array once.

#### Space Complexity:
- O(1): Only a constant amount of extra space is used.

### Binary Search Twice

#### Intuition:
To improve efficiency, consider using binary search. By performing two separate binary searches, we can find the first occurrence and the last occurrence of the target element. The key is to tailor the binary search condition to continue searching for the first or last position once a target element is found.

#### Code:
```java
public int[] searchRange(int[] nums, int target) {
    int first = findFirst(nums, target);
    int last = findLast(nums, target);
    return new int[]{first, last};
}

// Helper function to find the first occurrence
private int findFirst(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] >= target) {
            right = mid - 1;    // Move left to find the first occurrence
        } else {
            left = mid + 1;
        }
    }
    return (left < nums.length && nums[left] == target) ? left : -1;
}

// Helper function to find the last occurrence
private int findLast(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) {
            left = mid + 1;    // Move right to find the last occurrence
        } else {
            right = mid - 1;
        }
    }
    return (right >= 0 && nums[right] == target) ? right : -1;
}
```

#### Time Complexity:
- O(log n): Binary search takes logarithmic time.

#### Space Complexity:
- O(1): Only a constant amount of extra space is used.

### Optimized Binary Search

#### Intuition:
We can further optimize by combining the searches for the first and last occurrence into a single binary search. We check for both conditions in a single run, further reducing complexity in terms of constant factors. This method uses binary search with a twist by utilizing stricter compared conditions.

#### Code:
```java
public int[] searchRange(int[] nums, int target) {
    int start = -1, end = -1;
    
    // Perform a single binary search
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else {
            // Found target, now expand to find the bounds
            start = mid;
            end = mid;
            
            // Move in both directions to find the start and end
            while (start - 1 >= 0 && nums[start - 1] == target) {
                start--;
            }
            while (end + 1 < nums.length && nums[end + 1] == target) {
                end++;
            }
            return new int[]{start, end};
        }
    }
    
    return new int[]{start, end};
}
```

#### Time Complexity:
- O(log n) if elements are scattered, but potentially O(n) if all elements are the same and span both sections.

#### Space Complexity:
- O(1): Only a constant amount of extra space is used.

