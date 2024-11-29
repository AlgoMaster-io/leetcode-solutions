# 673. [Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approach 1: Dynamic Programming with Two Arrays

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
public class Solution {
    public int FindNumberOfLIS(int[] nums) {
        if (nums == null || nums.Length == 0) return 0;

        int n = nums.Length;
        int[] lengths = new int[n]; // lengths[i] will be the length of the longest ending in nums[i]
        int[] counts = new int[n];  // counts[i] will be the number of the longest ending in nums[i]
        Array.Fill(lengths, 1);
        Array.Fill(counts, 1);

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
        foreach (int length in lengths) {
            maxLength = Math.Max(maxLength, length);
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
csharp
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    class FenwickTree {
        int[] lengths, counts;

        public FenwickTree(int size) {
            lengths = new int[size];
            counts = new int[size];
        }

        public int[] Query(int index) {
            int maxLength = 0, totalCounts = 0;
            for (; index > 0; index -= index & -index) {
                if (lengths[index] > maxLength) {
                    maxLength = lengths[index];
                    totalCounts = counts[index];
                } else if (lengths[index] == maxLength) {
                    totalCounts += counts[index];
                }
            }
            return new int[] { maxLength, totalCounts };
        }

        public void Update(int index, int length, int count) {
            for (; index < lengths.Length; index += index & -index) {
                if (lengths[index] < length) {
                    lengths[index] = length;
                    counts[index] = count;
                } else if (lengths[index] == length) {
                    counts[index] += count;
                }
            }
        }
    }

    public int FindNumberOfLIS(int[] nums) {
        if (nums == null || nums.Length == 0) return 0;

        int offset = 1;
        SortedSet<int> set = new SortedSet<int>();
        foreach (int num in nums) set.Add(num);

        int rank = 0;
        Dictionary<int, int> numToIndex = new Dictionary<int, int>();
        foreach (int num in set) numToIndex[num] = ++rank;

        FenwickTree fenwickTree = new FenwickTree(rank + offset);
        int maxLength = 0, result = 0;

        foreach (int num in nums) {
            int currentIndex = numToIndex[num];
            int[] previous = fenwickTree.Query(currentIndex - 1);
            int currentLength = previous[0] + 1;
            int currentCount = Math.Max(previous[1], 1);

            fenwickTree.Update(currentIndex, currentLength, currentCount);

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

