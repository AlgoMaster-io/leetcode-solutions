# [Leetcode 164: Maximum Gap](https://leetcode.com/problems/maximum-gap/)

## Approaches
- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: Bucket Sort](#approach-2-bucket-sort)

### Approach 1: Sorting

**Intuition:**

The simplest way to find the maximum gap between consecutive elements is to first sort the array. Once the array is sorted, it's straightforward to iterate through the sorted list and find the maximum difference between consecutive elements.

Sorting provides us with the benefit of easily checking the difference between successive elements, which directly leads us to the maximum gap.

**Steps:**

1. If the number of elements in the array is less than two, the maximum gap is 0 since we need at least two numbers to define a gap.
2. Sort the array.
3. Initialize a variable to track the maximum gap.
4. Iterate over the sorted array and update the maximum gap whenever a larger gap is found between consecutive elements.
5. Return the maximum gap found.

**Time Complexity:**  
O(n log n) due to the sorting process.

**Space Complexity:**  
O(1) if the sorting can be done in-place or O(n) if we use a built-in sort that requires additional space.

```csharp
public class Solution {
    public int MaximumGap(int[] nums) {
        // If there are less than 2 numbers, we cannot have a gap
        if (nums.Length < 2) return 0;
        
        // Sort the array
        Array.Sort(nums);
        
        // Initialize maxGap to 0
        int maxGap = 0;
        
        // Iterate over sorted array to find the maximum gap
        for (int i = 1; i < nums.Length; i++) {
            // Calculate the gap between consecutive elements
            int gap = nums[i] - nums[i - 1];
            // Update maxGap if the current gap is larger
            maxGap = Math.Max(maxGap, gap);
        }
        
        // Return the largest gap found
        return maxGap;
    }
}
```

### Approach 2: Bucket Sort

**Intuition:**

To achieve a linear time solution, we can use a bucket sort-based technique. We aim to place numbers into buckets such that the maximum gap will only occur between buckets, not within them.

**Steps:**

1. Edge case check: If the number of elements is less than two, return 0.
2. Find the minimum (`min`) and maximum (`max`) numbers in the array.
3. Calculate the possible maximum gap (`gap`) using the formula:
   \[
   \text{gap} = \max(1, \lfloor \frac{\text{max} - \text{min}}{n - 1} \rfloor)
   \]
   where `n` is the length of the array.
4. Calculate the number of buckets `bucketCount = \frac{(\text{max} - \text{min})}{\text{gap}} + 1`.
5. Create an array of buckets where each bucket keeps track of the minimum and maximum elements that fall into the bucket.
6. Iterate over the array and place each element into its corresponding bucket by using:
   \[
   \text{bucketIndex} = \frac{(\text{num} - \text{min})}{\text{gap}}
   \]
7. Iterate over the buckets to calculate the maximum gap. The maximum gap will be the maximum difference between the current bucket's minimum and the previous bucket's maximum.
8. Return the maximum gap.

**Time Complexity:**  
O(n), where n is the number of elements in the input array since distributing the numbers into buckets and computing the maximum gap involves linear operations.

**Space Complexity:**  
O(n) due to additional space required for the buckets.

```csharp
public class Solution {
    public int MaximumGap(int[] nums) {
        if (nums.Length < 2) return 0;
        
        int min = nums.Min();
        int max = nums.Max();
        
        // Length of the gap based on maximum possible gap
        int gap = Math.Max(1, (max - min) / (nums.Length - 1));
        
        // Number of buckets
        int bucketCount = (max - min) / gap + 1;
        
        // Initialize buckets
        int[] bucketMin = new int[bucketCount];
        int[] bucketMax = new int[bucketCount];
        for (int i = 0; i < bucketCount; i++) {
            bucketMin[i] = int.MaxValue;
            bucketMax[i] = int.MinValue;
        }
        
        // Place each number in a bucket
        foreach (int num in nums) {
            int bucketIndex = (num - min) / gap;
            bucketMin[bucketIndex] = Math.Min(num, bucketMin[bucketIndex]);
            bucketMax[bucketIndex] = Math.Max(num, bucketMax[bucketIndex]);
        }
        
        // Calculate the maximum gap
        int maxGap = 0;
        int previousMax = min;
        for (int i = 0; i < bucketCount; i++) {
            if (bucketMin[i] == int.MaxValue) continue; // Bucket is empty
            maxGap = Math.Max(maxGap, bucketMin[i] - previousMax);
            previousMax = bucketMax[i];
        }
        
        return maxGap;
    }
}
```

In this solution, each number is placed into a bucket, and since the maximum gap will be between buckets, comparing the minimum and maximum values of consecutive buckets will yield the desired result.

