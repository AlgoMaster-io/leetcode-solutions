# [LeetCode 1095: Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array/)

## Solutions Overview
1. [Binary Search on Peak, then Binary Search on Ascending & Descending Part](#binary-search-on-peak)
2. [Optimized Single-pass Binary Search](#optimized-single-pass)

---

## Binary Search on Peak

### Intuition:
The problem requires finding a target in a specialized array structure called a "Mountain Array". A mountain array has an ascending and a descending segment where there is a peak element. First, find the peak element using binary search. Once the peak is found, perform binary search first on the ascending part, and if not found, then on the descending part.

### Approach:
1. **Find the Peak**:
   - Use binary search to find the peak.
   - Compare middle with its next element. If the middle is less than its next element, the peak lies on the right; otherwise, it lies on the left.

2. **Search the Ascending Part**:
   - Perform binary search on the portion from the start to the peak.
   - Standard binary search as this is a sorted (ascending) part.

3. **Search the Descending Part**:
   - If not found in the ascending part, perform binary search on the portion from the peak to the end.
   - Reverse binary search as this part is sorted in descending order.

### Code:
```javascript
function findInMountainArray(target, mountainArr) {
    const length = mountainArr.length();

    // Function to find the peak of the mountain array.
    function findPeak() {
        let left = 0, right = length - 1;
        while (left < right) {
            let mid = Math.floor((left + right) / 2);
            if (mountainArr.get(mid) < mountainArr.get(mid + 1)) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }

    // Function to perform binary search on ascending part.
    function binarySearch(left, right, target, asc = true) {
        while (left <= right) {
            let mid = Math.floor((left + right) / 2);
            let value = mountainArr.get(mid);

            if (value === target) return mid;
            if (asc) {
                if (value < target) left = mid + 1;
                else right = mid - 1;
            } else {
                if (value > target) left = mid + 1;
                else right = mid - 1;
            }
        }
        return -1;
    }

    const peak = findPeak();

    // Try to find the target in ascending order part
    let index = binarySearch(0, peak, target, true);
    if (index !== -1) return index;

    // Try to find the target in descending order part
    return binarySearch(peak + 1, length - 1, target, false);
}

```

### Complexity:
- **Time Complexity**: O(log n), where n is the number of elements in the mountain array. Each binary search step reduces the problem size by half.
- **Space Complexity**: O(1), as no extra space is used aside from variables.

---

## Optimized Single-pass

### Intuition:
By performing binary search directly on the entire mountain array, we can search for the target without first explicitly finding the peak. Adjust the direction of searches dynamically based on value comparisons.

### Approach:
1. Use two pointers to stay in the bounds of the array.
2. Instead of finding the peak first, check the middle value.
3. Depending on which part of the mountain array we are, adjust the next search direction dynamically:
   - If mid is greater than its next element, we could be in the descending part.
   - Use left-right comparisons to decide if the target might be on the current side of the mid.

### Code:
```javascript
function findInMountainArray(target, mountainArr) {
    const length = mountainArr.length();
    let left = 0, right = length - 1;

    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        const midValue = mountainArr.get(mid);
        
        if (midValue === target) return mid;
        
        // Determine if mid is on the ascending side of the peak or descending
        const isAscending = mountainArr.get(mid) < mountainArr.get(mid + 1);
        
        if (isAscending) {
            if (midValue < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        } else {
            if (midValue < target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
    }
    return -1;
}
```

### Complexity:
- **Time Complexity**: O(log n), as the search dynamically adjusts intervals.
- **Space Complexity**: O(1), because no additional space is utilized.

These solutions effectively find a target in a mountain array, leveraging binary search principles adjusted for the unique mountain structure.

