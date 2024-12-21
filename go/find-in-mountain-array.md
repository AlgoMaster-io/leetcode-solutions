# [Leetcode 1095: Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array/)

## Navigation
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Binary Search on each segment](#approach-2-binary-search-on-each-segment)
- [Approach 3: Peak Finding + Binary Search](#approach-3-peak-finding-binary-search)

## Approach 1: Brute Force

### Intuition
The simplest way to solve the problem is to iterate over the entire mountain array to find the target. We search linearly from index 0 to the peak and then from the peak to the end of the array.

### Code
```go
// Given an interface MountainArray { get(int index) int; length() int; }
func findInMountainArray(target int, mountainArr MountainArray) int {
    length := mountainArr.length()
    var peak int

    // find peak
    for peak = 1; peak < length-1; peak++ {
        if mountainArr.get(peak) > mountainArr.get(peak-1) && mountainArr.get(peak) > mountainArr.get(peak+1) {
            break
        }
    }

    // search in increasing part
    for i := 0; i <= peak; i++ {
        if mountainArr.get(i) == target {
            return i
        }
    }

    // search in decreasing part
    for i := peak + 1; i < length; i++ {
        if mountainArr.get(i) == target {
            return i
        }
    }

    return -1
}
```

### Complexity
- **Time Complexity:** O(n), where n is the number of elements in the mountain array since we may iterate the entire array.
- **Space Complexity:** O(1), as we do not use any extra space except a few variables.

## Approach 2: Binary Search on each segment

### Intuition
We can improve the solution by only performing binary search on the increasing and decreasing segments of the array. This reduces the complexity to O(log n) for each segment.

### Code
```go
// Binary search helper function
func binarySearch(arr MountainArray, start, end, target int, increasing bool) int {
    for start <= end {
        mid := start + (end - start) / 2
        midValue := arr.get(mid)

        if midValue == target {
            return mid
        }

        if increasing {
            if midValue < target {
                start = mid + 1
            } else {
                end = mid - 1
            }
        } else {
            if midValue < target {
                end = mid - 1
            } else {
                start = mid + 1
            }
        }
    }
    return -1
}

func findInMountainArray(target int, mountainArr MountainArray) int {
    length := mountainArr.length()
    var peak int

    // find peak using linear search as binary search requires additional checks, we'll improve this later.
    for peak = 1; peak < length-1; peak++ {
        if mountainArr.get(peak) > mountainArr.get(peak-1) && mountainArr.get(peak) > mountainArr.get(peak+1) {
            break
        }
    }

    // attempt binary search on the increasing sequence
    index := binarySearch(mountainArr, 0, peak, target, true)
    if index != -1 {
        return index
    }

    // attempt binary search on the decreasing sequence
    return binarySearch(mountainArr, peak+1, length-1, target, false)
}
```

### Complexity
- **Time Complexity:** O(log n), as we effectively perform binary search on each segment of the array.
- **Space Complexity:** O(1), as the additional space used is minimal.

## Approach 3: Peak Finding + Binary Search

### Intuition
We use a binary search approach to efficiently find the peak of the mountain array and then apply binary search on the two parts of the array (increasing and decreasing). This is the most efficient way to solve the problem.

### Code
```go
func findInMountainArray(target int, mountainArr MountainArray) int {
    length := mountainArr.length()

    // Function to find the peak index
    findPeakIndex := func(arr MountainArray) int {
        start, end := 0, length-1
        for start < end {
            mid := start + (end-start)/2
            if arr.get(mid) > arr.get(mid+1) {
                end = mid // descending part
            } else {
                start = mid + 1 // ascending part
            }
        }
        return start
    }

    peak := findPeakIndex(mountainArr)

    // Binary search on the increasing part
    index := binarySearch(mountainArr, 0, peak, target, true)
    if index != -1 {
        return index
    }

    // Binary search on the decreasing part
    return binarySearch(mountainArr, peak+1, length-1, target, false)
}
```

### Complexity
- **Time Complexity:** O(log n), finding the peak and performing binary search are both log n operations.
- **Space Complexity:** O(1), no extra space is needed except for a few variables.

