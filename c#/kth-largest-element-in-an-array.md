## [Leetcode 215: Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

### Approaches:
1. [Sorting Approach](#approach-1-sorting-approach)
2. [Heap Approach - Min-Heap](#approach-2-min-heap-approach)
3. [Quickselect Approach](#approach-3-quickselect-approach)

---

### Approach 1: Sorting Approach

#### Intuition:
The most straightforward way to find the k-th largest element in an array is by sorting the array first. Once sorted in descending order, the k-th element in the sorted array is the desired element. 

#### Code:
```csharp
public class Solution {
    public int FindKthLargest(int[] nums, int k) {
        // Step 1: Sort the array in descending order
        Array.Sort(nums, (a, b) => b.CompareTo(a));
        
        // Step 2: Return the k-th largest element
        return nums[k - 1];
    }
}
```

#### Time Complexity:
- Sorting the array takes \(O(n \log n)\) time, where \(n\) is the number of elements in the array.

#### Space Complexity:
- The space complexity is \(O(1)\) if the sorting can be done in place.

---

### Approach 2: Min-Heap Approach

#### Intuition:
Using a min-heap (or priority queue) is a more efficient approach in terms of time complexity when we are only concerned with the k largest elements. By maintaining a min-heap of size k, we can ensure that the top of the heap always contains the k-th largest element. As we iterate over the array, if the current element is larger than the root of the min-heap, we replace the root, effectively squeezing out smaller elements.

#### Code:
```csharp
using System.Collections.Generic;

public class Solution {
    public int FindKthLargest(int[] nums, int k) {
        // Create a min-heap with a priority queue data structure
        PriorityQueue<int, int> minHeap = new PriorityQueue<int, int>();

        // Iterate over each element in the array
        foreach (var num in nums) {
            // Add the current element to the heap
            minHeap.Enqueue(num, num);
            
            // If the heap grows larger than k, remove the smallest element
            if (minHeap.Count > k) {
                minHeap.Dequeue();
            }
        }
        
        // The k-th largest element will be at the root of the heap
        return minHeap.Dequeue();
    }
}
```

#### Time Complexity:
- Building the heap and maintaining it takes \(O(n \log k)\) time.

#### Space Complexity:
- Space complexity is \(O(k)\) for storing the elements in the heap.

---

### Approach 3: Quickselect Approach

#### Intuition:
The Quickselect algorithm is a selection algorithm related to quicksort. It works by partitioning the array around a pivot and recursively selecting the partition that contains the k-th largest element. This is similar to quicksort but only recursing into one of the subarrays.

#### Code:
```csharp
public class Solution {
    public int FindKthLargest(int[] nums, int k) {
        int n = nums.Length;
        return Quickselect(nums, 0, n - 1, n - k);
    }

    private int Quickselect(int[] nums, int left, int right, int k) {
        if (left == right) {
            // If the list contains only one element, return it
            return nums[left];
        }
        
        // Choose a pivot index randomly
        Random random = new Random();
        int pivotIndex = left + random.Next(right - left + 1);
        
        pivotIndex = Partition(nums, left, right, pivotIndex);
        
        if (k == pivotIndex) {
            // The pivot is in its final sorted position
            return nums[k];
        } else if (k < pivotIndex) {
            // Continue searching in the left part
            return Quickselect(nums, left, pivotIndex - 1, k);
        } else {
            // Continue searching in the right part
            return Quickselect(nums, pivotIndex + 1, right, k);
        }
    }

    private int Partition(int[] nums, int left, int right, int pivotIndex) {
        int pivot = nums[pivotIndex];
        // Move pivot to end
        Swap(nums, pivotIndex, right);
        
        int storeIndex = left;
        // Move all smaller elements to the left
        for (int i = left; i < right; i++) {
            if (nums[i] < pivot) {
                Swap(nums, storeIndex, i);
                storeIndex++;
            }
        }
        // Move pivot to its final place
        Swap(nums, storeIndex, right);
        
        return storeIndex;
    }

    private void Swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

#### Time Complexity:
- Average case: \(O(n)\)
- Worst case: \(O(n^2)\) (rare, can be avoided with a good pivot strategy)

#### Space Complexity:
- Space complexity is \(O(1)\) as it is an in-place algorithm. 

This quickselect method provides the optimal balance of performance and simplicity and is especially favored for large datasets.

