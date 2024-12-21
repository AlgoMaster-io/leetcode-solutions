# LeetCode Problem: [164. Maximum Gap](https://leetcode.com/problems/maximum-gap/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Sort and Check Approach](#sort-and-check-approach)
3. [Bucket Sort Approach (Most Optimal)](#bucket-sort-approach-most-optimal)

### Brute Force Approach

**Intuition**:  
The simplest approach is to consider all possible pairs of elements and calculate their absolute differences. By keeping track of the maximum difference, we can find the result. This approach does not leverage any optimizations and thus results in higher complexity.

```javascript
function maximumGap(nums) {
    if (nums.length < 2) return 0;
    let maxGap = 0;

    for (let i = 0; i < nums.length - 1; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            // Calculate absolute difference between nums[i] and nums[j]
            const gap = Math.abs(nums[i] - nums[j]);
            
            // Update maxGap if the current gap is larger
            maxGap = Math.max(maxGap, gap);
        }
    }

    return maxGap;
}
```
- **Time Complexity**: \(O(n^2)\), where \(n\) is the number of elements in the array.
- **Space Complexity**: \(O(1)\), no extra space is used besides variables for calculation.

### Sort and Check Approach

**Intuition**:  
Instead of checking all pairs, we can sort the array first and then only compute differences between adjacent elements. This reduces the number of pairs we need to consider and gives us a sorted order, making it efficient.

```javascript
function maximumGap(nums) {
    // If there are less than 2 elements, no gap can exist.
    if (nums.length < 2) return 0;

    // Sort the array to bring all close elements next to each other
    nums.sort((a, b) => a - b);

    let maxGap = 0;
    // Compare only adjacent elements for the maximum gap
    for (let i = 1; i < nums.length; i++) {
        const gap = nums[i] - nums[i - 1];
        maxGap = Math.max(maxGap, gap);
    }

    return maxGap;
}
```
- **Time Complexity**: \(O(n \log n)\), due to the sorting operation.
- **Space Complexity**: \(O(1)\) if in-place sorting is implemented; otherwise, \(O(n)\).

### Bucket Sort Approach (Most Optimal)

**Intuition**:  
The most optimal approach involves the "Bucketing" concept, leveraging the Pigeonhole Principle. By understanding that the maximum gap will be at least \((max\_num - min\_num) / (n - 1)\), we can divide the range of numbers into buckets and only compare maximum and minimum values across these buckets to find the maximum gap.

```javascript
function maximumGap(nums) {
    if (nums.length < 2) return 0;
    const n = nums.length;

    // Find the minimum and maximum value in the array
    let minNum = Math.min(...nums);
    let maxNum = Math.max(...nums);

    if (minNum === maxNum) return 0;

    // Calculate bucket size to ensure at least one bucket fits the range
    const bucketSize = Math.ceil((maxNum - minNum) / (n - 1));
    const bucketCount = Math.floor((maxNum - minNum) / bucketSize) + 1;

    // Prepare buckets, each bucket will contain the min and max of that bucket's range
    const buckets = Array.from({ length: bucketCount }, () => ({ min: Infinity, max: -Infinity }));

    // Place each number in a bucket
    nums.forEach(num => {
        const index = Math.floor((num - minNum) / bucketSize);
        buckets[index].min = Math.min(buckets[index].min, num);
        buckets[index].max = Math.max(buckets[index].max, num);
    });

    // Calculate the maximum gap by comparing the min of the current bucket to the max of the previous non-empty bucket
    let maxGap = 0;
    let previousMax = minNum; // Start with the smallest element in the array

    buckets.forEach(bucket => {
        if (bucket.min === Infinity) return;
        maxGap = Math.max(maxGap, bucket.min - previousMax);
        previousMax = bucket.max;
    });

    return maxGap;
}
```
- **Time Complexity**: \(O(n)\), traversing through the array and buckets is linear.
- **Space Complexity**: \(O(n)\), due to the space used by buckets.

