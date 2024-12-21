# [Leetcode 918: Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approaches:
1. [Brute Force](#brute-force)
2. [Kadane's Algorithm (Linear)](#kadanes-algorithm-linear)
3. [Kadane's Algorithm with Circular Consideration](#kadanes-algorithm-with-circular-consideration)

## Brute Force

### Intuition:
The brute force solution involves checking all possible subarrays by treating the array as circular where the end of the array wraps around to the beginning. For each starting point, we calculate the subarray sum by extending the end to cover each possible segment wrap-around in the array.

### Solution
```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& A) {
        int n = A.size();
        int maxSum = A[0];
        
        // Consider all subarrays starting from each index i
        for(int i = 0; i < n; ++i) {
            int currentSum = 0;
            // Accumulate sums by considering the array wrap-around using modulus
            for(int j = 0; j < n; ++j) {
                currentSum += A[(i + j) % n];
                maxSum = max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
};
```

### Complexity:
- **Time Complexity**: O(n^2), as we are checking all possible subarrays.
- **Space Complexity**: O(1), only using constant extra space.

## Kadane's Algorithm (Linear)

### Intuition:
Kadane's algorithm helps find the maximum subarray sum in linear time for a normal array. By considering the array as non-circular, we find the maximum subarray sum using the classic approach. This sets the foundation for solving the circular issue.

### Solution
```cpp
class Solution {
public:
    int kadanesAlgorithm(vector<int>& A) {
        int maxEndingHere = A[0], maxSoFar = A[0];
        for(int i = 1; i < A.size(); ++i) {
            // Extend or start new subarray
            maxEndingHere = max(A[i], maxEndingHere + A[i]);
            // Update max found so far
            maxSoFar = max(maxSoFar, maxEndingHere);
        }
        return maxSoFar;
    }
    
    int maxSubarraySumCircular(vector<int>& A) {
        return kadanesAlgorithm(A);
    }
};
```

### Complexity:
- **Time Complexity**: O(n), single iteration through the array.
- **Space Complexity**: O(1), using fixed extra space.

## Kadane's Algorithm with Circular Consideration

### Intuition:
To handle a circular array, we can consider two cases: 
1. When the maximum subarray sum is non-circular (using standard Kadane's).
2. When the maximum subarray sum is circular: Which can be visualized as total sum of the array minus the minimum subarray sum (ignoring the minimum subarray which will cause wrap-around).

### Solution
1. Calculate the maximum subarray sum using normal Kadane's algorithm.
2. Calculate the total sum of the array.
3. Calculate the minimum subarray sum using a variation of Kadane's algorithm.
4. The result is the maximum between:
   - Maximum subarray found in step 1.
   - Total sum - Minimum subarray sum in step 3.
5. Handle cases where all numbers are negatives specially by checking if all elements were contributing to the "wrap around".

```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& A) {
        int maxKadane = kadanesAlgorithm(A);
        
        int totalSum = 0;
        for(int& num : A) totalSum += num;
        
        // Invert the array to find minimum subarray sum using the same Kadane's algorithm
        int minKadane = kadanesAlgorithmMinimum(A);
        
        // If all numbers are negative, minKadane == totalSum, which means at least one number is negative
        if (minKadane == totalSum) return maxKadane;
        
        // Return the maximum of the non-circular and circular cases
        return max(maxKadane, totalSum - minKadane);
    }

private:
    int kadanesAlgorithm(vector<int>& A) {
        int maxEndingHere = A[0], maxSoFar = A[0];
        for(int i = 1; i < A.size(); ++i) {
            maxEndingHere = max(A[i], maxEndingHere + A[i]);
            maxSoFar = max(maxSoFar, maxEndingHere);
        }
        return maxSoFar;
    }
    
    int kadanesAlgorithmMinimum(vector<int>& A) {
        int minEndingHere = A[0], minSoFar = A[0];
        for(int i = 1; i < A.size(); ++i) {
            // Here we flip logic for finding minimum
            minEndingHere = min(A[i], minEndingHere + A[i]);
            minSoFar = min(minSoFar, minEndingHere);
        }
        return minSoFar;
    }
};
```

### Complexity:
- **Time Complexity**: O(n), as we are running the algorithm twice for maximum subarray and minimum subarray.
- **Space Complexity**: O(1), since we are using a constant amount of extra space.

