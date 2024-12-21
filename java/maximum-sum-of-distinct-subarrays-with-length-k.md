# [Leetcode 2461: Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

## Table of Contents
- [Approach 1: Sliding Window with Brute Force](#approach-1)
- [Approach 2: Sliding Window with HashSet (Optimized)](#approach-2)

### Approach 1: Sliding Window with Brute Force

#### Intuition
To find the maximum sum of distinct subarrays of a particular length \( K \), we can use a sliding window. However, a simple brute force approach involves generating all possible subarrays of length \( K \) and ensuring that each subarray consists of distinct elements. For each valid subarray, compute the sum and keep track of the maximum sum found.

#### Steps
1. Iterate over the array, building subarrays containing \( K \) elements.
2. For each subarray, check if all the elements are distinct.
3. If yes, compute the sum of the subarray. Update the maximum sum if it's larger than the current maximum.
4. Return the maximum sum found.

#### Code

```java
public class Solution {
    public int maximumSubarraySum(int[] nums, int k) {
        int n = nums.length;
        int maxSum = 0;
        
        for (int i = 0; i <= n - k; i++) {
            boolean[] visited = new boolean[10001];
            int sum = 0;
            boolean isDistinct = true;
            
            for (int j = i; j < i + k; j++) {
                if (visited[nums[j]]) {
                    isDistinct = false;
                    break;
                }
                visited[nums[j]] = true;
                sum += nums[j];
            }
            
            if (isDistinct) {
                maxSum = Math.max(maxSum, sum);
            }
        }
        
        return maxSum;
    }
}
```

#### Time Complexity
- \( O(n \cdot k) \), where \( n \) is the length of the input array.
- Checking distinctness within each subarray takes \( O(k) \).

#### Space Complexity
- \( O(1) \) additional space if we consider the size of `visited` as a constant.

### Approach 2: Sliding Window with HashSet (Optimized)

#### Intuition
An improved approach still uses a sliding window but leverages a HashSet to maintain a set of distinct elements in the current window. This avoids the need to check distinctness explicitly for each subarray by allowing additions and removals of elements from the window in constant time.

#### Steps
1. Maintain a sliding window with two pointers, \( left \) and \( right \).
2. Use a `HashSet` to maintain the distinct elements within the current window.
3. Iterate over the array and attempt to build a window of size \( K \).
4. If you've successfully built such a window with all distinct elements, compute the sum and update the maximum sum if necessary.
5. Move the window by adjusting \( left \).

#### Code

```java
import java.util.HashSet;

public class Solution {
    public int maximumSubarraySum(int[] nums, int k) {
        int n = nums.length;
        HashSet<Integer> set = new HashSet<>();
        int left = 0, right = 0, sum = 0, maxSum = 0;

        while (right < n) {
            // If adding the element keeps the window distinct
            while (set.contains(nums[right])) {
                // Remove element from the left
                set.remove(nums[left]);
                sum -= nums[left];
                left++;
            }

            // Add new element to the window
            set.add(nums[right]);
            sum += nums[right];
            right++;

            // Check if we reached the desired window size
            if (right - left == k) {
                maxSum = Math.max(maxSum, sum);
                // Move left to make room for the next element
                set.remove(nums[left]);
                sum -= nums[left];
                left++;
            }
        }

        return maxSum;
    }
}
```

#### Time Complexity
- \( O(n) \), where \( n \) is the length of the input array.

#### Space Complexity
- \( O(k) \) for storing elements in the `HashSet`.

