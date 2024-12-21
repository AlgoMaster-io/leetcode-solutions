# [LeetCode 918: Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approaches
1. [Kadane's Algorithm (Non-Circular Subarray)](#kadane-algorithm-non-circular-subarray)
2. [Maximum Sum of Circular Subarray Using Total Sum](#maximum-sum-of-circular-subarray-using-total-sum)

---

### Kadane's Algorithm (Non-Circular Subarray)

The initial step in solving this problem is to identify the maximum sum of a subarray in a non-circular sense using Kadane’s Algorithm. Here's the idea:

- Iterate through the array while maintaining a running sum of the subarray and the maximum subarray sum found so far.
- If the running sum becomes negative, reset it to zero as it won't contribute to a better maximum sum.

This solution considers only non-circular subarrays.

#### C# Code:
```csharp
public int MaxSubarraySumNonCircular(int[] A) {
    int maxEndingHere = 0, maxSoFar = A[0];
    foreach (var x in A) {
        // Add the current element to the running sum
        maxEndingHere = Math.Max(maxEndingHere + x, x);
        // Update the maximum sum encountered so far
        maxSoFar = Math.Max(maxSoFar, maxEndingHere);
    }
    return maxSoFar;
}
```

#### Time Complexity: 
- O(n), where n is the length of the array. We traverse the array only once.

#### Space Complexity: 
- O(1), since we are using only a fixed amount of space.

---

### Maximum Sum of Circular Subarray Using Total Sum

For a circular subarray, the maximum sum could be found by:

1. **Using Kadane's Algorithm** to find the maximum sum of a non-circular subarray.
2. Calculating the **total sum of the array**.
3. Finding the **minimum subarray sum** using the modification of Kadane’s Algorithm.
4. The maximum sum of a circular subarray would be the maximum of:
    - The max sum from Kadane’s (non-circular).
    - Total sum minus the min subarray sum (circular case).

**Note:**
- If all numbers are negative, the maximum subarray is simply the largest number (non-circular case).

#### C# Code:
```csharp
public int MaxSubarraySumCircular(int[] A) {
    // Step 1: Find the maximum subarray sum (non-circular)
    int maxKadane = MaxSubarraySumNonCircular(A);
    
    // Step 2: Calculate the total sum of the array
    int totalSum = 0;
    foreach (var num in A) {
        totalSum += num;
    }
    
    // Step 3: Find the minimum subarray sum
    int minEndingHere = 0, minSoFar = A[0];
    foreach (var num in A) {
        // Add the current element to the running sum
        minEndingHere = Math.Min(minEndingHere + num, num);
        // Update the minimum sum encountered so far
        minSoFar = Math.Min(minSoFar, minEndingHere);
    }
    
    // Step 4: Compute the maximum sum of the circular subarray
    int maxCircular = totalSum - minSoFar;
    
    // Step 5: Handle the edge case of all negative numbers
    return maxKadane > 0 ? Math.Max(maxKadane, maxCircular) : maxKadane;
}
```

#### Time Complexity: 
- O(n), since we traverse the array multiple times but within a linear order.

#### Space Complexity:
- O(1), as only a fixed number of variables are used regardless of input size.

---

