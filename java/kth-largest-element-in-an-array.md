[Leetcode Problem 215: Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

### Approaches:
1. [Basic Approach: Sorting](#basic-approach-sorting)
2. [Improved Approach: Min-Heap](#improved-approach-min-heap)
3. [Optimal Approach: Quickselect](#optimal-approach-quickselect)

---

## Basic Approach: Sorting

### Intuition:
The simplest and most direct way to find the k-th largest element is to sort the array first. Once sorted, the k-th largest can be easily accessed by indexing.

### Approach:
1. Sort the entire array in descending order.
2. Retrieve the k-th element from the sorted array.

### Code:
```java
import java.util.Arrays;

public class KthLargestElement {
    public int findKthLargest(int[] nums, int k) {
        // Sort the array in descending order
        Arrays.sort(nums);
        
        // The kth largest element will be at index (n-k) due to 0-indexing in Java arrays
        return nums[nums.length - k];
    }
}
```

### Complexity Analysis:
- **Time Complexity**: O(n log n) due to the sorting step.
- **Space Complexity**: O(1) if we sort in-place; otherwise, O(n) if we use additional space to store a sorted copy.

---

## Improved Approach: Min-Heap

### Intuition:
Instead of sorting the entire array, we can use a min-heap to maintain the k largest elements and efficiently find the k-th largest.

### Approach:
1. Create a min-heap of size k.
2. Iterate through each element of the array.
   - If the heap size is less than k, add the element to the heap.
   - Else if the current element is larger than the smallest element in the heap, replace the smallest.
3. The top of the heap will be the k-th largest element.

### Code:
```java
import java.util.PriorityQueue;

public class KthLargestElement {
    public int findKthLargest(int[] nums, int k) {
        // Min-heap with initial capacity k
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k);

        for (int num : nums) {
            // Add current number to heap
            minHeap.add(num);

            // If heap size exceeds k, remove the smallest element
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
        
        // The top of the min-heap is the kth largest element
        return minHeap.peek();
    }
}
```

### Complexity Analysis:
- **Time Complexity**: O(n log k), where n is the number of elements, as each insertion into the heap takes O(log k).
- **Space Complexity**: O(k) for the heap storing k elements.

---

## Optimal Approach: Quickselect

### Intuition:
Quickselect is a selection algorithm to find the k-th smallest element in an unordered list. It is based on the QuickSort algorithm. Instead of recursing both sides of the pivot, we only recurse into the part that contains the k-th element.

### Approach:
1. Use a partition method to place the pivot at its correct position in a sorted array.
2. If the pivot is at index `n-k`, return its value.
3. Otherwise, recurse on the subarray containing the k-th largest element.

### Code:
```java
import java.util.Random;

public class KthLargestElement {
    private Random rand = new Random();

    public int findKthLargest(int[] nums, int k) {
        // Start the quickselect process
        return quickselect(nums, 0, nums.length - 1, nums.length - k);
    }

    private int quickselect(int[] nums, int left, int right, int k) {
        if (left == right) {
            return nums[left];
        }
        
        // Randomly choose a pivot index
        int pivotIndex = left + rand.nextInt(right - left + 1);
        pivotIndex = partition(nums, left, right, pivotIndex);
        
        // If the pivot index matches k, found the k-th largest element
        if (pivotIndex == k) {
            return nums[k];
        } else if (pivotIndex < k) {
            // Search on the right side
            return quickselect(nums, pivotIndex + 1, right, k);
        } else {
            // Search on the left side
            return quickselect(nums, left, pivotIndex - 1, k);
        }
    }
    
    private int partition(int[] nums, int left, int right, int pivotIndex) {
        int pivotValue = nums[pivotIndex];
        swap(nums, pivotIndex, right); // Move pivot to end
        int storeIndex = left;

        for (int i = left; i < right; i++) {
            if (nums[i] < pivotValue) {
                swap(nums, storeIndex, i);
                storeIndex++;
            }
        }
        swap(nums, right, storeIndex); // Move pivot to its final place
        return storeIndex;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

### Complexity Analysis:
- **Time Complexity**: O(n) on average, O(n^2) in the worst case due to the pivot choice.
- **Space Complexity**: O(1) since we perform in-place partitioning.

This concludes the various approaches to solve the problem finding the k-th largest element in an array, improving from sorting to efficient quickselect.

