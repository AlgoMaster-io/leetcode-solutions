# [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approaches
1. [Brute Force using Sorting](#brute-force-using-sorting)
2. [Min-Heap for Optimal Solution](#min-heap-for-optimal-solution)

---

### Brute Force using Sorting

**Intuition:**

The simple and direct way to keep track of the kth largest element in a stream is to store all elements and sort them each time a new element is added. From the sorted list, we can directly access the kth largest element.

**Approach:**

1. Initialize a list to store all the elements of the stream.
2. Whenever a new element is added:
   - Add this element to the list.
   - Sort the list.
   - Return the `k`th largest element, which is the element located at the `length-k` index.
   
**Time Complexity:**  
- For adding an element: \(O(n \log n)\) due to sorting, where \(n\) is the number of elements in the list.
- Finding the kth largest element: \(O(1)\).

**Space Complexity:**  
- \(O(n)\) for storing all the elements.

```csharp
public class KthLargest {
    private int k;
    private List<int> nums;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        this.nums = nums.ToList();
    }

    public int Add(int val) {
        nums.Add(val);
        nums.Sort(); // Sorting the list
        return nums[nums.Count - k]; // Getting the kth largest element
    }
}
```

---

### Min-Heap for Optimal Solution

**Intuition:**

The most efficient way to maintain the kth largest element in a stream is to use a min-heap. A min-heap of size `k` keeps the largest `k` elements encountered so far, and the smallest element in this heap is the kth largest element from the stream.

**Approach:**

1. Use a min-heap (priority queue).
2. Initialize by adding the first `k` elements to the heap.
3. For any new element:
   - If the heap size is less than `k`, simply add the element to the heap.
   - Otherwise, compare the new element with the minimum (root) of the heap. If the new element is larger, replace the root with this new element and adjust the heap.
4. The root element of the heap is the kth largest element.

**Time Complexity:**  
- Adding an element: \(O(\log k)\) because the size of the heap is maintained at `k`.
- Finding the kth largest element: \(O(1)\).

**Space Complexity:**  
- \(O(k)\) for storing the elements in the heap.

```csharp
using System.Collections.Generic;

public class KthLargest {
    private int k;
    private PriorityQueue<int, int> minHeap;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        minHeap = new PriorityQueue<int, int>();

        foreach (var num in nums)
            Add(num);
    }

    public int Add(int val) {
        if (minHeap.Count < k) {
            minHeap.Enqueue(val, val); // Add new element if not enough elements in the heap
        } else if (val > minHeap.Peek()) {
            minHeap.Dequeue();  // Remove the smallest element
            minHeap.Enqueue(val, val); // Add the new element as it is larger
        }

        return minHeap.Peek(); // Root of the min-heap is the kth largest element
    }
}
```

This min-heap approach is optimal given the constraints of maintaining the kth largest element in a dynamic stream.

