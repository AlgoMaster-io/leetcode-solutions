# 673. [Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approach 1: Dynamic Programming with Two Arrays

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n)
public class Solution {
    public int findNumberOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int n = nums.length;
        int[] lengths = new int[n]; // lengths[i] will be the length of the longest ending in nums[i]
        int[] counts = new int[n];  // counts[i] will be the number of the longest ending in nums[i]
        Arrays.fill(lengths, 1);
        Arrays.fill(counts, 1);

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    if (lengths[j] + 1 > lengths[i]) {
                        lengths[i] = lengths[j] + 1;
                        counts[i] = counts[j];
                    } else if (lengths[j] + 1 == lengths[i]) {
                        counts[i] += counts[j];
                    }
                }
            }
        }

        int maxLength = 0, numberOfLIS = 0;
        for (int length : lengths) {
            maxLength = Math.max(maxLength, length);
        }
        for (int i = 0; i < n; i++) {
            if (lengths[i] == maxLength) {
                numberOfLIS += counts[i];
            }
        }

        return numberOfLIS;
    }
}
```

## Approach 2: Optimized Dynamic Programming with Fenwick Tree

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import java.util.*;

public class Solution {
    class FenwickTree {
        int[] lengths, counts;

        public FenwickTree(int size) {
            lengths = new int[size];
            counts = new int[size];
        }

        public int[] query(int index) {
            int maxLength = 0, totalCounts = 0;
            for (; index > 0; index -= index & -index) {
                if (lengths[index] > maxLength) {
                    maxLength = lengths[index];
                    totalCounts = counts[index];
                } else if (lengths[index] == maxLength) {
                    totalCounts += counts[index];
                }
            }
            return new int[]{maxLength, totalCounts};
        }

        public void update(int index, int length, int count) {
            for (; index < lengths.length; index += index & -index) {
                if (lengths[index] < length) {
                    lengths[index] = length;
                    counts[index] = count;
                } else if (lengths[index] == length) {
                    counts[index] += count;
                }
            }
        }
    }

    public int findNumberOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        
        int offset = 1;
        Set<Integer> set = new TreeSet<>();
        for (int num : nums) set.add(num);

        int rank = 0;
        Map<Integer, Integer> numToIndex = new HashMap<>();
        for (int num : set) numToIndex.put(num, ++rank);

        FenwickTree fenwickTree = new FenwickTree(rank + offset);
        int maxLength = 0, result = 0;

        for (int num : nums) {
            int currentIndex = numToIndex.get(num);
            int[] previous = fenwickTree.query(currentIndex - 1);
            int currentLength = previous[0] + 1;
            int currentCount = Math.max(previous[1], 1);

            fenwickTree.update(currentIndex, currentLength, currentCount);

            if (currentLength > maxLength) {
                maxLength = currentLength;
                result = currentCount;
            } else if (currentLength == maxLength) {
                result += currentCount;
            }
        }

        return result;
    }
}
```


