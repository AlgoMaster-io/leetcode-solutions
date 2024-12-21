# [Leetcode 164: Maximum Gap](https://leetcode.com/problems/maximum-gap/)

### Approaches
- [Approach 1: Simple Sort and Compare](#approach-1-simple-sort-and-compare)
- [Approach 2: Bucket Sort](#approach-2-bucket-sort)

## Approach 1: Simple Sort and Compare

### Intuition
The simplest way to solve this problem is to sort the array and then find the maximum gap between successive elements. Although this is not the most optimal solution in terms of time complexity, it is straightforward to implement and understand.

### Steps
1. Sort the given array.
2. Initialize a variable `maxGap` to store the maximum difference found.
3. Iterate through the sorted array and calculate the difference between successive elements.
4. Update `maxGap` if a larger difference is found.

### Time Complexity
- **O(N log N)** due to sorting the array.

### Space Complexity
- **O(1)** if the sort is done in place.

### Java Code
```java
import java.util.Arrays;

public class MaximumGap {
    public int maximumGap(int[] nums) {
        if (nums.length < 2) return 0;

        // Step 1: Sort the array
        Arrays.sort(nums);

        // Step 2: Initialize maxGap as minimum possible value
        int maxGap = 0;

        // Step 3: Iterate over the array to find the maximum gap
        for (int i = 1; i < nums.length; i++) {
            // Calculate the gap between successive elements
            int gap = nums[i] - nums[i - 1];
            // Update maxGap if the current gap is larger
            maxGap = Math.max(maxGap, gap);
        }

        return maxGap;
    }
}
```

## Approach 2: Bucket Sort

### Intuition
To achieve a better time complexity, we can use a bucket sort method. The core idea is distributing the numbers into buckets such that each bucket holds a range of numbers. We calculate the maximum possible difference, which ensures that the maximum gap cannot be confined within a single bucket but must be between the maximum of one bucket and the minimum of the next.

### Steps
1. Find the minimum and maximum elements in the array.
2. Calculate the gap size, which is the ceiling value of `(max - min) / (n - 1)`.
3. Create buckets that will store only the minimum and maximum values that fall into them.
4. Iterate through each number to place it in the right bucket by calculating the appropriate index.
5. Finally, iterate through the buckets to calculate the maximum gap between the maximum value of the current non-empty bucket and the minimum value of the next non-empty bucket.

### Time Complexity
- **O(N)** for both average and worst case due to the linear pass to fill buckets and the linear pass to find the maximum gap.

### Space Complexity
- **O(N)** due to the space used for the buckets.

### Java Code
```java
public class MaximumGap {
    public int maximumGap(int[] nums) {
        if (nums.length < 2) return 0;
        
        // Find minimum and maximum values in the array
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        for (int num : nums) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }

        // If all numbers are identical, the maximum gap is zero
        if (min == max) return 0;

        int n = nums.length;
        // Step 2: Calculate the bucket size using the gap
        int bucketSize = Math.max(1, (max - min) / (n - 1));
        int bucketCount = (max - min) / bucketSize + 1;
        
        // Step 3: Create buckets
        int[] bucketMin = new int[bucketCount];
        int[] bucketMax = new int[bucketCount];
        Arrays.fill(bucketMin, Integer.MAX_VALUE);
        Arrays.fill(bucketMax, Integer.MIN_VALUE);
        
        // Step 4: Place numbers in the appropriate buckets
        for (int num : nums) {
            int bucketIndex = (num - min) / bucketSize;
            bucketMin[bucketIndex] = Math.min(bucketMin[bucketIndex], num);
            bucketMax[bucketIndex] = Math.max(bucketMax[bucketIndex], num);
        }

        // Step 5: Calculate the maximum gap
        int maxGap = 0, prevMax = min;
        for (int i = 0; i < bucketCount; i++) {
            // Ignore empty buckets
            if (bucketMin[i] == Integer.MAX_VALUE) continue;
            // Update the maximum gap
            maxGap = Math.max(maxGap, bucketMin[i] - prevMax);
            prevMax = bucketMax[i];
        }

        return maxGap;
    }
}
```

This approach, leveraging bucket sorting, achieves linear time complexity by effectively managing the largest gaps between consecutive buckets. Each bucket captures the range of numbers it could potentially span, thus ensuring efficient lookup and gap calculation.

