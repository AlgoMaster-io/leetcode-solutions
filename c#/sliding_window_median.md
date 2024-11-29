# 480. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approach 1: Two Heaps 

### Solution
java
```java
// Time Complexity: O(n log k)
// Space Complexity: O(k)
import java.util.PriorityQueue;
import java.util.Collections;

public class Solution {
    private PriorityQueue<Integer> minHeap;
    private PriorityQueue<Integer> maxHeap;
    
    public double[] medianSlidingWindow(int[] nums, int k) {
        minHeap = new PriorityQueue<>(); // Min-heap for the larger half
        maxHeap = new PriorityQueue<>(Collections.reverseOrder()); // Max-heap for the smaller half
        double[] result = new double[nums.length - k + 1];

        for (int i = 0; i < nums.length; i++) {
            if (maxHeap.isEmpty() || nums[i] <= maxHeap.peek()) {
                maxHeap.offer(nums[i]);
            } else {
                minHeap.offer(nums[i]);
            }
            
            // Balance the heaps
            if (maxHeap.size() > minHeap.size() + 1) {
                minHeap.offer(maxHeap.poll());
            } else if (minHeap.size() > maxHeap.size()) {
                maxHeap.offer(minHeap.poll());
            }
            
            // Start to form the window
            if (i >= k - 1) {
                // Append the median to the result
                if (k % 2 == 0) {
                    result[i - k + 1] = ((double)maxHeap.peek() + minHeap.peek()) / 2;
                } else {
                    result[i - k + 1] = maxHeap.peek();
                }
                
                // Remove the element that's sliding out of the window
                int elementToRemove = nums[i - k + 1];
                if (elementToRemove <= maxHeap.peek()) {
                    maxHeap.remove(elementToRemove);
                } else {
                    minHeap.remove(elementToRemove);
                }
                
                // Balance the heaps after removal
                if (maxHeap.size() > minHeap.size() + 1) {
                    minHeap.offer(maxHeap.poll());
                } else if (minHeap.size() > maxHeap.size()) {
                    maxHeap.offer(minHeap.poll());
                }
            }
        }
        
        return result;
    }
}
```

csharp
```csharp
// Time Complexity: O(n log k)
// Space Complexity: O(k)
using System;
using System.Collections.Generic;

public class Solution {
    private PriorityQueue<int, int> minHeap;
    private PriorityQueue<int, int> maxHeap;
    
    public double[] MedianSlidingWindow(int[] nums, int k) {
        minHeap = new PriorityQueue<int, int>(); // Min-heap for the larger half
        maxHeap = new PriorityQueue<int, int>(Comparer<int>.Create((a, b) => b.CompareTo(a))); // Max-heap for the smaller half
        double[] result = new double[nums.Length - k + 1];

        for (int i = 0; i < nums.Length; i++) {
            if (maxHeap.Count == 0 || nums[i] <= maxHeap.Peek()) {
                maxHeap.Enqueue(nums[i], nums[i]);
            } else {
                minHeap.Enqueue(nums[i], nums[i]);
            }
            
            // Balance the heaps
            if (maxHeap.Count > minHeap.Count + 1) {
                minHeap.Enqueue(maxHeap.Dequeue(), maxHeap.Peek());
            } else if (minHeap.Count > maxHeap.Count) {
                maxHeap.Enqueue(minHeap.Dequeue(), minHeap.Peek());
            }
            
            // Start to form the window
            if (i >= k - 1) {
                // Append the median to the result
                if (k % 2 == 0) {
                    result[i - k + 1] = ((double)maxHeap.Peek() + minHeap.Peek()) / 2;
                } else {
                    result[i - k + 1] = maxHeap.Peek();
                }
                
                // Remove the element that's sliding out of the window
                int elementToRemove = nums[i - k + 1];
                if (elementToRemove <= maxHeap.Peek()) {
                    maxHeap = RemoveElement(maxHeap, elementToRemove);
                } else {
                    minHeap = RemoveElement(minHeap, elementToRemove);
                }
                
                // Balance the heaps after removal
                if (maxHeap.Count > minHeap.Count + 1) {
                    minHeap.Enqueue(maxHeap.Dequeue(), maxHeap.Peek());
                } else if (minHeap.Count > maxHeap.Count) {
                    maxHeap.Enqueue(minHeap.Dequeue(), minHeap.Peek());
                }
            }
        }
        
        return result;
    }
    
    private PriorityQueue<int, int> RemoveElement(PriorityQueue<int, int> heap, int element) {
        var buffer = new List<int>();
        while (heap.Count > 0) {
            int current = heap.Dequeue();
            if (current != element) {
                buffer.Add(current);
            } else {
                break;
            }
        }
        foreach (var item in buffer) {
            heap.Enqueue(item, item);
        }
        return heap;
    }
}
```

