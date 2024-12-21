# [Find in Mountain Array - LeetCode](https://leetcode.com/problems/find-in-mountain-array/)

## Solutions:
- [Approach 1: Bruteforce Search](#approach-1-bruteforce-search)
- [Approach 2: Binary Search (Find Peak + Search on Both Halves)](#approach-2-binary-search-find-peak-search-on-both-halves)
- [Approach 3: Optimized Binary Search](#approach-3-optimized-binary-search)

### Approach 1: Bruteforce Search

#### Intuition:
In this approach, we traverse the entire array sequentially to find the target value. As the mountain array has an increasing then decreasing nature, any sequence search should be able to spot the target if present.

#### Steps:
1. Iterate through the array to find if any element matches the target.
2. If found, return the index, else return -1.

```csharp
// A MountainArray interface is provided as per problem constraint
int BruteforceSearch(MountainArray mountainArr, int target) {
    int length = mountainArr.Length();
    for (int i = 0; i < length; i++) {
        if (mountainArr.Get(i) == target) {
            return i; // return the index where target is found
        }
    }
    return -1; // return -1 if target is not found in the array
}
```

- **Time Complexity:** O(n), where n is the number of elements in the array.
- **Space Complexity:** O(1).

### Approach 2: Binary Search (Find Peak + Search on Both Halves)

#### Intuition:
A mountain array increases to a peak then decreases. By finding the peak, we can decide where to perform a binary search: left or right of the peak.

#### Steps:
1. Find the peak of the mountain array using binary search.
2. Perform binary search on the increasing left side.
3. Perform binary search on the decreasing right side if the target isn't found on the left side.

```csharp
int FindInMountainArray(int target, MountainArray mountainArr) {
    int peak = FindPeak(mountainArr);
    int result = AscendingBinarySearch(mountainArr, target, 0, peak);
    if (result != -1) {
        return result;
    }
    return DescendingBinarySearch(mountainArr, target, peak + 1, mountainArr.Length() - 1);
}

int FindPeak(MountainArray mountainArr) {
    int left = 0, right = mountainArr.Length() - 1;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (mountainArr.Get(mid) < mountainArr.Get(mid + 1)) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return left; // Peak found
}

int AscendingBinarySearch(MountainArray mountainArr, int target, int left, int right) {
    while (left <= right) {
        int mid = left + (right - left) / 2;
        int value = mountainArr.Get(mid);
        if (value == target) {
            return mid; // target found in the left portion
        } else if (value < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1; // target not found in the left portion
}

int DescendingBinarySearch(MountainArray mountainArr, int target, int left, int right) {
    while (left <= right) {
        int mid = left + (right - left) / 2;
        int value = mountainArr.Get(mid);
        if (value == target) {
            return mid; // target found in the right portion
        } else if (value > target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1; // target not found in the right portion
}
```

- **Time Complexity:** O(log n), due to three binary searches (one for peak, one on each half).
- **Space Complexity:** O(1).

### Approach 3: Optimized Binary Search

#### Intuition:
Directly integrating peak finding and searching, minimizing the need for array reads, and balancing calls within a limited number of reads given by `Get`.

#### Steps:
1. Similar peak finding is performed.
2. Combine the peak finding and immediate checks around the found peak, execute a more integrated search with fewer `Get` operations.

**Note:** Due to the constraints on `MountainArray` operations, this approach doesn't differ much from the previous but optimizes reads.

- For the sake of brevity and clarity, implementation remains akin to Approach 2 but with a focus on precision of execution and integration.

Given the nature of this problem and constraints on operations through `MountainArray`, approaches like 2 are quite optimal. Consider tuning the search strategies if performance is a concern or constraints are adjusted.

