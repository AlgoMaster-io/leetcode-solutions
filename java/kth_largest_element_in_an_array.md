# [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approach 1: Sorting

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(1)
import java.util.Arrays;

public class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums); // Sort the array in ascending order
        return nums[nums.length - k]; // Return the kth largest element
    }
}
```

## Approach 2: Using a Min-Heap

### Solution
```java
// Time Complexity: O(n log k)
// Space Complexity: O(k)
import java.util.PriorityQueue;

public class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(); // Min-heap to keep the top k elements

        for (int num : nums) {
            minHeap.offer(num); // Add the current number to the heap
            if (minHeap.size() > k) {
                minHeap.poll(); // Remove the smallest element if the heap size exceeds k
            }
        }

        return minHeap.peek(); // The root of the heap is the kth largest element
    }
}
```

## Approach 3: Quickselect (Partitioning)

### Solution
```java
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1)
import java.util.Random;

public class Solution {
    public int findKthLargest(int[] nums, int k) {
        int n = nums.length;
        return quickSelect(nums, 0, n - 1, n - k);
    }

    private int quickSelect(int[] nums, int left, int right, int k) {
        if (left == right) {
            return nums[left]; // Base case: only one element
        }

        Random random = new Random();
        int pivotIndex = left + random.nextInt(right - left + 1);

        pivotIndex = partition(nums, left, right, pivotIndex);

        if (pivotIndex == k) {
            return nums[k]; // Found the kth largest element
        } else if (pivotIndex < k) {
            return quickSelect(nums, pivotIndex + 1, right, k); // Search in the right part
        } else {
            return quickSelect(nums, left, pivotIndex - 1, k); // Search in the left part
        }
    }

    private int partition(int[] nums, int left, int right, int pivotIndex) {
        int pivotValue = nums[pivotIndex];
        swap(nums, pivotIndex, right); // Move pivot to the end
        int storeIndex = left;

        for (int i = left; i < right; i++) {
            if (nums[i] < pivotValue) {
                swap(nums, storeIndex, i);
                storeIndex++;
            }
        }

        swap(nums, storeIndex, right); // Move pivot to its final position
        return storeIndex;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```