# [Leetcode 918: Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approaches:
- [Approach 1: Basic Kadane's Algorithm](#approach-1-basic-kadanes-algorithm)
- [Approach 2: Maximum Subarray Sum by Reversing with Kadane's](#approach-2-maximum-subarray-sum-by-reversing-with-kadanes)

---

## Approach 1: Basic Kadane's Algorithm

### Intuition:
The basic approach to solve the Maximum Subarray Sum problem can be achieved by using Kadane's Algorithm. However, this problem introduces a circular array aspect: that is, the end of the array wraps around to the start. We need to find the maximum subarray in two scenarios: one without the wrapping, and one where the subarray might wrap around the end of the array back to the beginning.

### Steps:
1. **Without Circularity:** Use Kadane's algorithm to find the maximum subarray sum that does not wrap around (the traditional approach).
2. **Handle Wrap-around:** The other possible case is that the maximum subarray wraps around the circle. This can be handled by considering the total array sum minus the minimum subarray sum (found via Kadane's applied to the *negative* values).
3. Handle the edge case where all numbers are negative: in this case, the maximum subarray is just a single (least negative) number, so we should not consider wrapping.

### Code:

```java
public int maxSubarraySumCircular(int[] nums) {
    int totalSum = 0; 
    int maxSumWithoutWrap = Integer.MIN_VALUE; 
    int currentMax = 0; 
    int minSumWithWrap = Integer.MAX_VALUE; 
    int currentMin = 0;
    
    for (int num : nums) {
        totalSum += num;
        
        // Calculate max without wrap using standard Kadane’s algorithm
        currentMax = Math.max(currentMax + num, num);
        maxSumWithoutWrap = Math.max(maxSumWithoutWrap, currentMax);
        
        // Calculate min sum using modified Kadane’s to find wrap-around
        currentMin = Math.min(currentMin + num, num);
        minSumWithWrap = Math.min(minSumWithWrap, currentMin);
    }
    
    // Edge case: All numbers are negative
    if (maxSumWithoutWrap < 0) {
        return maxSumWithoutWrap;
    }
    
    return Math.max(maxSumWithoutWrap, totalSum - minSumWithWrap);
}
```

### Complexity:
- **Time Complexity:** O(N), where N is the number of elements in the array, due to single pass traversal.
- **Space Complexity:** O(1), as no extra space is utilized.

---

## Approach 2: Maximum Subarray Sum by Reversing with Kadane's

### Intuition:
To optimize the Kadane algorithm with circularity consideration, instead of finding the maximum wrapping subarray by reversing the sum, we can exploit the summation properties. The idea is finding the complement of the subarray that gives a minimum sum will help deduce the potential maximum subarray that spans the array circularly.

### Steps:
1. Compute the total array sum.
2. Identify the sum of the minimum subarray using Kadane's on negatives.
3. Compare the straightforward Kadane's approach with the sum derived from subtracting the minimum subarray from the total sum.

This version is essentially the same as the first, but emphasizes inversion of thinking for circular subarray handling.

### Code remains the same (as logic doesn’t change):

```java
public int maxSubarraySumCircular(int[] nums) {
    int totalSum = 0; 
    int maxSumWithoutWrap = Integer.MIN_VALUE; 
    int currentMax = 0; 
    int minSumWithWrap = Integer.MAX_VALUE; 
    int currentMin = 0;
    
    for (int num : nums) {
        totalSum += num;
        
        // Calculate max without wrap using standard Kadane’s algorithm
        currentMax = Math.max(currentMax + num, num);
        maxSumWithoutWrap = Math.max(maxSumWithoutWrap, currentMax);
        
        // Calculate min sum using modified Kadane’s to find wrap-around
        currentMin = Math.min(currentMin + num, num);
        minSumWithWrap = Math.min(minSumWithWrap, currentMin);
    }
    
    // Edge case: All numbers are negative
    if (maxSumWithoutWrap < 0) {
        return maxSumWithoutWrap;
    }
    
    return Math.max(maxSumWithoutWrap, totalSum - minSumWithWrap);
}
```

### Complexity:
- **Time Complexity:** O(N), because it involves linear scans over the list.
- **Space Complexity:** O(1), all operations are done in-place.

---
As the problem progresses through understanding the use of summed reversals for the circular case, these approaches efficiently handle potential subarray wrapping concerns, leveraging Kadane’s algorithm both directly and indirectly.

