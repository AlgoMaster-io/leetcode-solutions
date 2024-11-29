# 528. [Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approach 1: Linear Search for Picking (Alternative to Binary Search)

### Solution
```java
// Time Complexity: O(n) for initialization, O(n) for each pick
// Space Complexity: O(n)
import java.util.Random;

public class Solution {
    private int[] prefixSums;
    private Random random;

    public Solution(int[] w) {
        prefixSums = new int[w.length];
        random = new Random();

        // Build the prefix sum array
        prefixSums[0] = w[0];
        for (int i = 1; i < w.length; i++) {
            prefixSums[i] = prefixSums[i - 1] + w[i];
        }
    }

    public int pickIndex() {
        // Generate a random number between 1 and the total sum
        int target = random.nextInt(prefixSums[prefixSums.length - 1]) + 1;

        // Perform linear search to find the target index
        for (int i = 0; i < prefixSums.length; i++) {
            if (target <= prefixSums[i]) {
                return i; // Return the first index with a prefix sum >= target
            }
        }

        return -1; // Should not reach here
    }
}
```

## Approach 2: Prefix Sum Array and Binary Search

### Solution
```java
// Time Complexity: O(n) for initialization, O(log n) for each pick
// Space Complexity: O(n)
import java.util.Random;

public class Solution {
    private int[] prefixSums;
    private Random random;

    public Solution(int[] w) {
        prefixSums = new int[w.length];
        random = new Random();

        // Build the prefix sum array
        prefixSums[0] = w[0];
        for (int i = 1; i < w.length; i++) {
            prefixSums[i] = prefixSums[i - 1] + w[i];
        }
    }

    public int pickIndex() {
        // Generate a random number between 1 and the total sum
        int target = random.nextInt(prefixSums[prefixSums.length - 1]) + 1;

        // Use binary search to find the target index
        int start = 0, end = prefixSums.length - 1;
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (prefixSums[mid] < target) {
                start = mid + 1; // Narrow search to the right half
            } else {
                end = mid; // Narrow search to the left half
            }
        }

        return start; // Return the index corresponding to the random number
    }
}
```