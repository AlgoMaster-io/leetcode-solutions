# [Leetcode 528: Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approaches
- [Approach 1: Prefix Sum + Linear Search](#approach-1-prefix-sum--linear-search)
- [Approach 2: Prefix Sum + Binary Search for Improved Efficiency](#approach-2-prefix-sum--binary-search-for-improved-efficiency)

## Approach 1: Prefix Sum + Linear Search

### Intuition
The main idea is to convert the problem of picking a number with weight into a cumulative sum problem. By maintaining a cumulative prefix sum array, each weighted index indicates a higher chance of being picked proportionally to its weight. This is achieved by generating a random number within the range of the total weight and finding its correspondent prefix sum index.

### Steps:
1. **Calculate Prefix Sums:** Create a prefix sum array where each element `i` in the array contains the sum of all weights from `0` to `i`.
2. **Total Weight:** The last element in the prefix sum array gives the total weight, which is the range for generating random numbers.
3. **Linear Search:** Generate a random number within this total weight and perform a linear search on the prefix sum array to determine the index.

### Code
```csharp
public class Solution {
    private int[] prefixSums;
    private Random random;
    
    public Solution(int[] w) {
        prefixSums = new int[w.Length];
        prefixSums[0] = w[0];
        
        // Create cumulative sum array
        for (int i = 1; i < w.Length; i++) {
            prefixSums[i] = prefixSums[i - 1] + w[i];
        }
        
        random = new Random();
    }
    
    public int PickIndex() {
        // Get a random value
        int target = random.Next(1, prefixSums[prefixSums.Length - 1] + 1);
        
        // Find the target in the prefix sums using linear search
        for (int i = 0; i < prefixSums.Length; i++) {
            if (target <= prefixSums[i]) {
                return i;
            }
        }
        return -1; // Should never reach here
    }
}
```

### Complexity
- **Time Complexity:** O(n) for the prefix sum calculation, O(n) for each pick using linear search.
- **Space Complexity:** O(n) to store the prefix sum array.

## Approach 2: Prefix Sum + Binary Search for Improved Efficiency

### Intuition
The linear search can be improved using binary search due to the sorted nature of the prefix sum array. By leveraging binary search, we efficiently locate the target index in a faster manner, reducing the pick operation complexity from O(n) to O(log n).

### Steps:
1. **Same as Approach 1** for creating prefix sum and calculating total weight.
2. **Binary Search:** Use binary search on the prefix sum array to find the smallest index where the prefix sum is greater than or equal to the target random number.

### Code
```csharp
public class Solution {
    private int[] prefixSums;
    private Random random;
    
    public Solution(int[] w) {
        prefixSums = new int[w.Length];
        prefixSums[0] = w[0];
        
        // Create cumulative sum array
        for (int i = 1; i < w.Length; i++) {
            prefixSums[i] = prefixSums[i - 1] + w[i];
        }
        
        random = new Random();
    }
    
    public int PickIndex() {
        // Get a random value
        int target = random.Next(1, prefixSums[prefixSums.Length - 1] + 1);
        
        // Binary search for the first index where prefixSum >= target
        int left = 0, right = prefixSums.Length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (prefixSums[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

### Complexity
- **Time Complexity:** O(n) for the prefix sum calculation, O(log n) for each pick using binary search.
- **Space Complexity:** O(n) to store the prefix sum array.

