# [Problem: 1095. Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array/)

## Table of Contents
- [Approach 1: Binary Search for Peak + Linear Scan](#approach-1-binary-search-for-peak--linear-scan)
- [Approach 2: Modified Binary Search](#approach-2-modified-binary-search)

### Approach 1: Binary Search for Peak + Linear Scan

#### Intuition:
The problem can be tackled by first locating the peak element in the mountain array. Once the peak is found, there are two parts separated by the peak - an increasing subsequence and a decreasing subsequence. With the peak known, a straightforward method is to scan both halves of the array to find the target.

1. **Find the Peak:** Use a binary search to find the peak of the mountain.
2. **Linear Scan:** After identifying the peak, perform a linear scan on both sides of the peak to find the target.

#### Code:
```cpp
class MountainArray {
public:
    int get(int index);
    int length();
};

class Solution {
public:
    int findInMountainArray(int target, MountainArray &mountainArr) {
        int length = mountainArr.length();
        
        // Find the peak index of the mountain
        int peak = findPeakIndex(mountainArr, length);
        
        // Try to find target in the increasing part
        int index = binarySearch(mountainArr, target, 0, peak, true);
        if (index != -1) {
            return index;
        }
        
        // Try to find target in the decreasing part
        return binarySearch(mountainArr, target, peak + 1, length - 1, false);
    }
    
    int findPeakIndex(MountainArray &arr, int length) {
        int low = 0, high = length - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (arr.get(mid) < arr.get(mid + 1)) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
    
    int binarySearch(MountainArray &arr, int target, int low, int high, bool ascending) {
        while (low <= high) {
            int mid = low + (high - low) / 2;
            int midValue = arr.get(mid);
            if (midValue == target) {
                return mid;
            }
            
            if (ascending) {
                if (midValue < target) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            } else {
                if (midValue < target) {
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            }
        }
        return -1;
    }
};
```

#### Time Complexity:
- **O(log n)**: Finding the peak is O(log n) due to binary search. Each binary search on either side of the peak is also O(log n).

#### Space Complexity:
- **O(1)**: No additional space is used apart from variables and recursion stack space.

### Approach 2: Modified Binary Search

#### Intuition:
The above approach still requires two separate searches after finding the peak. A more optimal way is to integrate the peak finding and target search into one pass.

1. **Integrated Binary Search**: Perform a modified binary search from the start, adjusting search operation as the array increases until mid, and vice-versa.

#### Code:
```cpp
class Solution {
public:
    int findInMountainArray(int target, MountainArray &mountainArr) {
        int left = 0, right = mountainArr.length() - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (mountainArr.get(mid) < mountainArr.get(mid + 1))
                left = mid + 1;
            else
                right = mid;
        }
        
        int peak = left;

        // Binary search for target in the ascending part up to the peak
        int index = binarySearch(mountainArr, target, 0, peak, true);
        if (index != -1) {
            return index;
        }

        // Binary search for target in the descending part after the peak
        return binarySearch(mountainArr, target, peak + 1, mountainArr.length() - 1, false);
    }
    
    int binarySearch(MountainArray &arr, int target, int low, int high, bool ascending) {
        while (low <= high) {
            int mid = low + (high - low) / 2;
            int midValue = arr.get(mid);
            if (midValue == target) {
                return mid;
            }
            
            if (ascending) {
                if (midValue < target) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            } else {
                if (midValue < target) {
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            }
        }
        return -1;
    }
};
```

#### Time Complexity:
- **O(log n)**: Efficiently locates the peak and identifies the target through binary searches.

#### Space Complexity:
- **O(1)**: Utilizes constant space for operations excluding input size.

