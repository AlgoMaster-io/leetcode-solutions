# [Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

- [Approach 1: Brute Force](#approach-1)
- [Approach 2: Sliding Window with Set](#approach-2)
- [Approach 3: Optimal Sliding Window with Hash Map](#approach-3)

## Approach 1: Brute Force

### Intuition:
The simplest solution involves checking every possible subarray of length `k` and calculating its sum, ensuring all elements in the subarray are distinct. This is inefficient due to repeatedly checking the same conditions.

### Steps:
1. Iterate through all possible subarrays of length `k`.
2. For each subarray, check if it contains all distinct elements.
3. Calculate the sum of the subarray if all elements are distinct, otherwise move to the next subarray.
4. Keep track of the maximum sum found.

### Code:
```typescript
function maximumSumDistinctSubarrayK(nums: number[], k: number): number {
    let maxSum = 0;

    // iterate over all possible windows
    for (let i = 0; i <= nums.length - k; i++) {
        const subarray = nums.slice(i, i + k);
        const distinctElements = new Set(subarray);

        // Check if the current subarray contains all distinct elements
        if (distinctElements.size === k) {
            const currentSum = subarray.reduce((acc, num) => acc + num, 0);
            maxSum = Math.max(maxSum, currentSum);
        }
    }

    return maxSum;
}
```

### Complexity:
- **Time Complexity:** \(O(n \times k)\), where `n` is the length of the array and `k` is the size of the subarray. We check every k-length subarray for uniqueness.
- **Space Complexity:** \(O(k)\), for storing elements in the set for each subarray.

## Approach 2: Sliding Window with Set

### Intuition:
Use the sliding window technique to avoid recalculating from scratch by removing the first element of the previous window and adding the next element of the current window. However, sets aren't efficient for removal operations, which necessitates a better approach.

### Steps:
1. Use a sliding window technique with a set to track distinct elements within the window.
2. Slide the window across the array, updating sums and checking for distinct elements efficiently.

### Code:
```typescript
function maximumSumDistinctSubarrayK(nums: number[], k: number): number {
    let maxSum = 0;
    let currentSum = 0;
    let left = 0;
    const set = new Set<number>();

    for (let right = 0; right < nums.length; right++) {
        // Slide right of window
        while (set.has(nums[right])) {
            // Remove elements from the left until `nums[right]` is unique in the set
            set.delete(nums[left]);
            currentSum -= nums[left];
            left++;
        }

        // Add new unique element
        set.add(nums[right]);
        currentSum += nums[right];

        // If the window is of size k determine max sum
        if (right - left + 1 === k) {
            maxSum = Math.max(maxSum, currentSum);
            set.delete(nums[left]);
            currentSum -= nums[left];
            left++;
        }
    }

    return maxSum;
}
```

### Complexity:
- **Time Complexity:** \(O(n)\), the loop iterates over the array once and the set operations are all \(O(1)\) on average.
- **Space Complexity:** \(O(k)\), as the set can contain at most `k` elements.

## Approach 3: Optimal Sliding Window with Hash Map

### Intuition:
Use a hash map to track the count of each element. This allows efficient adding and removing of elements while preserving the distinct count status.

### Steps:
1. Maintain a hash map to track occurrences of elements in the window.
2. Slide the window while updating the map and calculate the sum when the subarray has distinct elements.
3. Ensure the window maintains exactly `k` elements.

### Code:
```typescript
function maximumSumDistinctSubarrayK(nums: number[], k: number): number {
    let maxSum = 0;
    let currentSum = 0;
    let left = 0;
    const countMap = new Map<number, number>();

    for (let right = 0; right < nums.length; right++) {
        const element = nums[right];
        currentSum += element;
        countMap.set(element, (countMap.get(element) || 0) + 1);

        // If window size exceeds `k`, shrink from the left
        if (right - left + 1 > k) {
            const leftElement = nums[left];
            currentSum -= leftElement;
            // Decrement or remove from map
            if (countMap.get(leftElement) === 1) {
                countMap.delete(leftElement);
            } else {
                countMap.set(leftElement, countMap.get(leftElement)! - 1);
            }
            left++;
        }
        
        // Check if current subarray is valid
        if (countMap.size === k) {
            maxSum = Math.max(maxSum, currentSum);
        }
    }

    return maxSum;
}
```

### Complexity:
- **Time Complexity:** \(O(n)\), single pass through the array with constant time map operations.
- **Space Complexity:** \(O(k)\), for storing elements and their counts in the hash map.

