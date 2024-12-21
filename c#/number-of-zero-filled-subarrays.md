# [Leetcode 2348: Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointers (Optimized)](#approach-2-two-pointers-optimized)

---

## Approach 1: Brute Force
### Intuition
The simplest way to analyze the problem is to consider all possible subarrays of the given array and count those which are entirely filled with zeroes. Though this method is computationally costly, it helps to understand the problem better.

### Steps
- Iterate over all possible subarrays using a nested loop.
- For each subarray, check if it is filled only with zeroes.
- Count subarrays that meet the zero-filled condition.

### Code
```csharp
public class Solution {
    public long ZeroFilledSubarray(int[] nums) {
        long count = 0;
        
        // Outer loop to set the starting point
        for (int i = 0; i < nums.Length; i++) {
            // Inner loop to set the ending point
            for (int j = i; j < nums.Length; j++) {
                // Assume current subarray is zero-filled
                bool isZeroFilled = true;
                
                // Check if all elements in current subarray are zero
                for (int k = i; k <= j; k++) {
                    if (nums[k] != 0) {
                        isZeroFilled = false;
                        break;
                    }
                }
                
                // If zero-filled, increase count
                if (isZeroFilled) {
                    count++;
                }
            }
        }
        
        return count;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n^3), where n is the length of the array. We have a triple nested loop.
- **Space Complexity**: O(1), no additional space is used.

---

## Approach 2: Two Pointers (Optimized)
### Intuition
In a more optimal approach, we can use the concept of counting consecutive zeroes. For each zero we encounter, the number of zero-filled subarrays increases by the number of contiguous zeroes seen so far. This way, we skip over non-zero numbers quickly without checking each possible subarray explicitly.

### Steps
- Iterate through the array with a single pointer.
- Use a counter to keep track of consecutive zeroes.
- Whenever a zero is found, increase the count of zeroes and add it to the result.
- If a non-zero is encountered, reset the consecutive zeroes counter.

### Code
```csharp
public class Solution {
    public long ZeroFilledSubarray(int[] nums) {
        long count = 0;
        long zeroes = 0;
        
        // Iterate over each element in array
        for (int i = 0; i < nums.Length; i++) {
            if (nums[i] == 0) {
                // Increase the zero streak count
                zeroes++;
                // Add all subarrays ending at `i` with all zeroes
                count += zeroes;
            } else {
                // Reset the zero streak on non-zero number
                zeroes = 0;
            }
        }
        
        return count;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the array. We iterate through the array once.
- **Space Complexity**: O(1), only constant space is required.


