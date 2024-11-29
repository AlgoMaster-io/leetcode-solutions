# 315. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approach 1: Brute Force

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public IList<int> CountSmaller(int[] nums) {
        var result = new List<int>();
        
        // Traverse each element and count smaller numbers to its right
        for (int i = 0; i < nums.Length; i++) {
            int count = 0;
            for (int j = i + 1; j < nums.Length; j++) {
                if (nums[j] < nums[i]) {
                    count++;
                }
            }
            result.Add(count);
        }
        
        return result;
    }
}
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
csharp
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<int> CountSmaller(int[] nums) {
        int[] result = new int[nums.Length];
        int[] sortedNums = (int[])nums.Clone();
        
        // Sort and remove duplicates from nums
        Array.Sort(sortedNums);
        int[] fenwickTree = new int[nums.Length + 1];
        
        // Traverse nums from right to left
        for (int i = nums.Length - 1; i >= 0; i--) {
            int rank = Array.BinarySearch(sortedNums, nums[i]) + 1;
            result[i] = Query(rank - 1, fenwickTree);
            Update(rank, 1, fenwickTree);
        }
        
        // Convert result array to list
        var resultList = new List<int>(result);
        return resultList;
    }
    
    // Update the Fenwick Tree
    private void Update(int index, int value, int[] fenwickTree) {
        while (index < fenwickTree.Length) {
            fenwickTree[index] += value;
            index += index & -index;
        }
    }

    // Query the prefix sum in the Fenwick Tree
    private int Query(int index, int[] fenwickTree) {
        int sum = 0;
        while (index > 0) {
            sum += fenwickTree[index];
            index -= index & -index;
        }
        return sum;
    }
}
```

## Approach 3: Merge Sort

### Solution
csharp
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    private int[] counts;

    public IList<int> CountSmaller(int[] nums) {
        counts = new int[nums.Length];
        int[] indices = new int[nums.Length];
        
        // Initialize indices
        for (int i = 0; i < nums.Length; i++) {
            indices[i] = i;
        }
        
        // Perform merge sort
        MergeSort(nums, indices, 0, nums.Length - 1);
        
        // Convert counts array to list
        var resultList = new List<int>(counts);
        return resultList;
    }
    
    // Perform merge sort and count smaller elements
    private void MergeSort(int[] nums, int[] indices, int left, int right) {
        if (left >= right) return;
        
        int mid = left + (right - left) / 2;
        MergeSort(nums, indices, left, mid);
        MergeSort(nums, indices, mid + 1, right);
        
        Merge(nums, indices, left, mid, right);
    }
    
    // Merge two sorted halves and update counts
    private void Merge(int[] nums, int[] indices, int left, int mid, int right) {
        int[] tempIndices = new int[right - left + 1];
        int i = left, j = mid + 1, k = 0, smallerCount = 0;
        
        // Merge the two halves
        while (i <= mid && j <= right) {
            if (nums[indices[i]] <= nums[indices[j]]) {
                counts[indices[i]] += smallerCount;
                tempIndices[k++] = indices[i++];
            } else {
                smallerCount++;
                tempIndices[k++] = indices[j++];
            }
        }
        
        // Copy remaining elements
        while (i <= mid) {
            counts[indices[i]] += smallerCount;
            tempIndices[k++] = indices[i++];
        }
        while (j <= right) {
            tempIndices[k++] = indices[j++];
        }
        
        // Copy merged result back to indices
        for (int m = 0; m < tempIndices.Length; m++) {
            indices[left + m] = tempIndices[m];
        }
    }
}
```

