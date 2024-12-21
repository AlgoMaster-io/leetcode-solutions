# LeetCode Problem 1095: Find in Mountain Array

**Link to Problem:** [LeetCode 1095 - Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array/)

## Navigation

- [Approach 1: Brute Force Linear Search](#approach-1-brute-force-linear-search)
- [Approach 2: Optimized Binary Search](#approach-2-optimized-binary-search)

---

## Approach 1: Brute Force Linear Search

### Intuition:
In a mountain array, there exists a peak element after which elements start decreasing. For a brute force approach, one can simply iterate over the entire array to find the target. This involves two phases:
1. Traverse from the start to the peak (increasing sequence).
2. Traverse from the peak to the end (decreasing sequence).

### Steps:
1. Iterate through the array from the start to the peak to look for the target.
2. If not found, iterate from the peak to the end.

### Time Complexity:
- **O(n)**: As we may have to look at each element in the worst case twice. 

### Space Complexity:
- **O(1)**: No additional space used apart from variables.

### Java Code:

```java
class Solution {
    public int findInMountainArray(int target, MountainArray mountainArr) {
        int n = mountainArr.length();
        
        // Linear search on increasing part
        int peak = 0;
        for (int i = 0; i < n; i++) {
            if (mountainArr.get(i) > mountainArr.get(peak)) {
                peak = i;
            }
            if (mountainArr.get(i) == target) {
                return i;
            }
        }
        
        // Linear search on decreasing part.
        for (int i = n - 1; i > peak; i--) {
            if (mountainArr.get(i) == target) {
                return i;
            }
        }
        
        // If the target is not found
        return -1;
    }
}
```

---

## Approach 2: Optimized Binary Search

### Intuition:
Utilizing the properties of the mountain array, we can apply binary search methods to efficiently find the peak and then search for the target in both increasing and decreasing sequences separately.

### Steps:
1. Use a binary search to find the peak of the mountain array.
2. Perform a binary search on the increasing sequence from the start to the peak to find the target.
3. If the target is not found, perform a binary search on the decreasing sequence from the peak to the end.

### Time Complexity:
- **O(log n)**: The binary search is applied three times (once to find peak and twice to find target).

### Space Complexity:
- **O(1)**: We are not using any extra data structures.

### Java Code:

```java
class Solution {
    public int findInMountainArray(int target, MountainArray mountainArr) {
        int n = mountainArr.length();
        
        // Step 1: Find the peak index using binary search
        int left = 0, right = n - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (mountainArr.get(mid) < mountainArr.get(mid + 1)) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        int peak = left;
        
        // Step 2: Binary search for target in the increasing part
        int index = binarySearch(mountainArr, target, 0, peak, true);
        if (index != -1) {
            return index;
        }
        
        // Step 3: Binary search for target in the decreasing part
        return binarySearch(mountainArr, target, peak + 1, n - 1, false);
    }
    
    private int binarySearch(MountainArray arr, int target, int left, int right, boolean isAsc) {
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int midVal = arr.get(mid);
            if (midVal == target) {
                return mid;
            }
            if (isAsc) {
                if (midVal < target) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            } else {
                if (midVal > target) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        return -1;
    }
}
```

In this optimal approach, we efficiently leverage the binary search technique to locate the target using the mountain array's unique properties, achieving a log-scaled complexity.

