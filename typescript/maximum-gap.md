# [Leetcode Problem 164: Maximum Gap](https://leetcode.com/problems/maximum-gap/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sorting](#approach-2-sorting)
- [Approach 3: Radix Sort and Pigeonhole Principle](#approach-3-radix-sort-and-pigeonhole-principle)

## Approach 1: Brute Force

### Intuition

In this approach, we calculate the difference between every pair of elements in the array and keep track of the maximum difference. This brute force approach can be useful for understanding the problem, but it is not efficient for larger input sizes.

### Code

```typescript
function maximumGap(nums: number[]): number {
    if (nums.length < 2) return 0;
    
    let maxGap = 0;
    // Iterate over every possible pair
    for (let i = 0; i < nums.length - 1; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            // Calculate and compare the gap between current pair
            maxGap = Math.max(maxGap, Math.abs(nums[i] - nums[j]));
        }
    }
    
    return maxGap;
}
```

### Complexity Analysis

- **Time Complexity:** \(O(n^2)\) - Due to the nested loop iterating through every possible pair.
- **Space Complexity:** \(O(1)\) - No additional space is needed.

## Approach 2: Sorting

### Intuition

A more optimal approach involves sorting the array first, then finding the maximum gap by comparing adjacent elements. Sorting helps because the maximum gap will definitely be between sorted successive elements.

### Code

```typescript
function maximumGap(nums: number[]): number {
    if (nums.length < 2) return 0;

    // Sort the numbers
    nums.sort((a, b) => a - b);
    let maxGap = 0;

    // Iterate over sorted numbers, compute gaps between consecutive elements
    for (let i = 1; i < nums.length; i++) {
        maxGap = Math.max(maxGap, nums[i] - nums[i - 1]);
    }

    return maxGap;
}
```

### Complexity Analysis

- **Time Complexity:** \(O(n \log n)\) - Dominated by the sort operation.
- **Space Complexity:** \(O(1)\) - Sorting in-place if allowed, otherwise \(O(n)\).

## Approach 3: Radix Sort and Pigeonhole Principle

### Intuition

For an even more optimal solution, consider a non-comparison-based sorting technique like Radix Sort. This can sort the numbers in linear time. We can use a bucket-like strategy knowing that the max possible gap is distributed across these sorted numbers.

### Code

```typescript
function maximumGap(nums: number[]): number {
    if (nums.length < 2) return 0;

    const n = nums.length;
    let maxNum = Math.max(...nums);
    let exp = 1;
    let buffer = new Array(n);
    
    // Radix sort based on each digit position
    while (maxNum / exp > 0) {
        const count = new Array(10).fill(0);

        // Count occurrences
        for (let num of nums) {
            const digit = Math.floor(num / exp) % 10;
            count[digit]++;
        }

        // Accumulate count
        for (let i = 1; i < 10; i++) {
            count[i] += count[i - 1];
        }

        // Place elements in a temporary buffer based on accumulated counts
        for (let i = n - 1; i >= 0; i--) {
            const num = nums[i];
            const digit = Math.floor(num / exp) % 10;
            buffer[--count[digit]] = num;
        }

        // Copy sorted elements back to the original array
        for (let i = 0; i < n; i++) {
            nums[i] = buffer[i];
        }

        exp *= 10;
    }
    
    let maxGap = 0;
    // Find the maximum gap between consecutive sorted elements
    for (let i = 1; i < n; i++) {
        maxGap = Math.max(maxGap, nums[i] - nums[i - 1]);
    }

    return maxGap;
}
```

### Complexity Analysis

- **Time Complexity:** \(O(n)\) - Radix sort's linear time complexity with respect to the number of input digits.
- **Space Complexity:** \(O(n)\) - Additional space for the buffer used in sorting.

By employing Radix Sort and leveraging the distribution of numbers efficiently, we can achieve optimal performance. This approach ideally suits scenarios where input constraints are broader.

