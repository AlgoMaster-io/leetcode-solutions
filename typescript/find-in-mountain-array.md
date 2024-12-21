# [Leetcode 1095: Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Binary Search (Two-Pass)](#approach-2-binary-search-two-pass)
- [Approach 3: Optimized Binary Search](#approach-3-optimized-binary-search)

---

## Approach 1: Brute Force

### Intuition
The mountain array is composed of a strictly increasing sequence followed by a strictly decreasing sequence. The brute force approach involves iterating through the array to find the peak (the largest element) and then linearly searching for the target in both the increasing and decreasing parts of the array. This approach is straightforward but inefficient, especially with larger arrays.

### Implementation

```typescript
function findInMountainArray(target: number, mountainArr: MountainArray): number {
    const length: number = mountainArr.length();
    
    // Step 1: Find the peak element
    let peakIndex = 0;
    for (let i = 1; i < length; i++) {
        if (mountainArr.get(i) > mountainArr.get(i - 1)) {
            peakIndex = i;
        } else {
            break;
        }
    }
    
    // Step 2: Search in the increasing part of the mountain
    for (let i = 0; i <= peakIndex; i++) {
        if (mountainArr.get(i) === target) {
            return i;
        }
    }
    
    // Step 3: Search in the decreasing part of the mountain
    for (let i = peakIndex + 1; i < length; i++) {
        if (mountainArr.get(i) === target) {
            return i;
        }
    }
    
    return -1; // Target not found
}
```

### Complexity
- **Time Complexity**: \(O(n)\) where \(n\) is the length of the array. We traverse the array linearly.
- **Space Complexity**: \(O(1)\) since we are using constant extra space.

---

## Approach 2: Binary Search (Two-Pass)

### Intuition
The key observation is that the array is divided into two parts with distinct properties. We can leverage binary search to efficiently find the peak first. Once the peak is identified, we apply binary search separately to the two halves. This reduces the time complexity significantly.

### Implementation

```typescript
function findInMountainArray(target: number, mountainArr: MountainArray): number {
    const length: number = mountainArr.length();

    // Step 1: Find the peak index using binary search
    let start = 0, end = length - 1;
    while (start < end) {
        const mid = Math.floor((start + end) / 2);
        if (mountainArr.get(mid) < mountainArr.get(mid + 1)) {
            start = mid + 1;
        } else {
            end = mid;
        }
    }
    const peakIndex = start;

    // Step 2: Binary search in the increasing part
    start = 0, end = peakIndex;
    while (start <= end) {
        const mid = Math.floor((start + end) / 2);
        const midValue = mountainArr.get(mid);
        if (midValue === target) {
            return mid;
        } else if (midValue < target) {
            start = mid + 1;
        } else {
            end = mid - 1;
        }
    }

    // Step 3: Binary search in the decreasing part
    start = peakIndex + 1, end = length - 1;
    while (start <= end) {
        const mid = Math.floor((start + end) / 2);
        const midValue = mountainArr.get(mid);
        if (midValue === target) {
            return mid;
        } else if (midValue > target) {
            start = mid + 1;
        } else {
            end = mid - 1;
        }
    }

    return -1; // Target not found
}
```

### Complexity
- **Time Complexity**: \(O(\log n)\) for finding the peak and \(O(\log n)\) for the binary searches, totaling \(O(\log n)\).
- **Space Complexity**: \(O(1)\), using constant extra space.

---

## Approach 3: Optimized Binary Search

### Intuition
In this approach, we optimize the search by combining the peak finding and search operation. By determining bounds during the search process, the peak and target searching can be interleaved, thus potentially reducing unnecessary checks.

### Implementation

```typescript
function findInMountainArray(target: number, mountainArr: MountainArray): number {
    const length: number = mountainArr.length();

    // Combined peak finding and target searching
    let start = 0, end = length - 1;
    while (start < end) {
        const mid = Math.floor((start + end) / 2);
        const midValue = mountainArr.get(mid);
        const midNextValue = mountainArr.get(mid + 1);
        
        if (midValue < midNextValue) {
            start = mid + 1; // We're on the increasing slope
        } else {
            end = mid; // We're on the decreasing slope
        }
    }

    const peakIndex = start;

    // Function to perform binary search
    const binarySearch = (left: number, right: number, ascending: boolean): number => {
        while (left <= right) {
            const mid = Math.floor((left + right) / 2);
            const midValue = mountainArr.get(mid);

            if (midValue === target) {
                return mid;
            }

            if (ascending) {
                if (midValue < target) left = mid + 1;
                else right = mid - 1;
            } else {
                if (midValue > target) left = mid + 1;
                else right = mid - 1;
            }
        }
        return -1;
    }

    // Search in the ascending part
    let result = binarySearch(0, peakIndex, true);
    if (result !== -1) return result;
    
    // Search in the descending part
    result = binarySearch(peakIndex + 1, length - 1, false);

    return result;
}
```

### Complexity
- **Time Complexity**: \(O(\log n)\) for finding the peak and performing binary searches.
- **Space Complexity**: \(O(1)\) using constant extra space.

This solution optimizes the process by integrating peak and search phases. Each solution builds on the previous one to improve efficiency while maintaining readability and utilizing specific characteristics of the mountain array structure.

