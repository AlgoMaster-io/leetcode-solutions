# 315. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approach 1: Brute Force

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n)
import java.util.List;
import java.util.ArrayList;

public class Solution {
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> result = new ArrayList<>();
        
        // Traverse each element and count smaller numbers to its right
        for (int i = 0; i < nums.length; i++) {
            int count = 0;
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] < nums[i]) {
                    count++;
                }
            }
            result.add(count);
        }
        
        return result;
    }
}
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import java.util.List;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;

public class Solution {
    public List<Integer> countSmaller(int[] nums) {
        int[] result = new int[nums.length];
        int[] sortedNums = nums.clone();
        
        // Sort and remove duplicates from nums
        Arrays.sort(sortedNums);
        int[] fenwickTree = new int[nums.length + 1];
        
        // Traverse nums from right to left
        for (int i = nums.length - 1; i >= 0; i--) {
            int rank = Arrays.binarySearch(sortedNums, nums[i]) + 1;
            result[i] = query(rank - 1, fenwickTree);
            update(rank, 1, fenwickTree);
        }
        
        // Convert result array to list
        List<Integer> resultList = new ArrayList<>();
        for (int count : result) {
            resultList.add(count);
        }
        
        return resultList;
    }
    
    // Update the Fenwick Tree
    private void update(int index, int value, int[] fenwickTree) {
        while (index < fenwickTree.length) {
            fenwickTree[index] += value;
            index += index & -index;
        }
    }

    // Query the prefix sum in the Fenwick Tree
    private int query(int index, int[] fenwickTree) {
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
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import java.util.List;
import java.util.ArrayList;
import java.util.Arrays;

public class Solution {
    private int[] counts;

    public List<Integer> countSmaller(int[] nums) {
        counts = new int[nums.length];
        int[] indices = new int[nums.length];
        
        // Initialize indices
        for (int i = 0; i < nums.length; i++) {
            indices[i] = i;
        }
        
        // Perform merge sort
        mergeSort(nums, indices, 0, nums.length - 1);
        
        // Convert counts array to list
        List<Integer> resultList = new ArrayList<>();
        for (int count : counts) {
            resultList.add(count);
        }
        
        return resultList;
    }
    
    // Perform merge sort and count smaller elements
    private void mergeSort(int[] nums, int[] indices, int left, int right) {
        if (left >= right) return;
        
        int mid = left + (right - left) / 2;
        mergeSort(nums, indices, left, mid);
        mergeSort(nums, indices, mid + 1, right);
        
        merge(nums, indices, left, mid, right);
    }
    
    // Merge two sorted halves and update counts
    private void merge(int[] nums, int[] indices, int left, int mid, int right) {
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
        System.arraycopy(tempIndices, 0, indices, left, tempIndices.length);
    }
}
```

