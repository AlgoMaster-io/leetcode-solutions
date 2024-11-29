# 480. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approach: Two Heaps (Max-Heap and Min-Heap)

### Solution
```java
// Time Complexity: O(n * log(k)), where n is the size of nums and k is the size of the window
// Space Complexity: O(k), for storing elements in the heaps
import java.util.PriorityQueue;
import java.util.Comparator;

public class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        double[] result = new double[n - k + 1];
        
        // Max-heap for the smaller half
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Comparator.reverseOrder());
        // Min-heap for the larger half
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        
        for (int i = 0; i < n; i++) {
            // Add the new element into the appropriate heap
            if (maxHeap.isEmpty() || nums[i] <= maxHeap.peek()) {
                maxHeap.add(nums[i]);
            } else {
                minHeap.add(nums[i]);
            }
            
            // Rebalance the heaps if necessary
            if (maxHeap.size() > minHeap.size() + 1) {
                minHeap.add(maxHeap.poll());
            } else if (minHeap.size() > maxHeap.size()) {
                maxHeap.add(minHeap.poll());
            }
            
            // Remove the element that is sliding out of the window
            if (i >= k) {
                int elementToRemove = nums[i - k];
                if (elementToRemove <= maxHeap.peek()) {
                    maxHeap.remove(elementToRemove);
                } else {
                    minHeap.remove(elementToRemove);
                }
                
                // Rebalance the heaps after removal
                if (maxHeap.size() > minHeap.size() + 1) {
                    minHeap.add(maxHeap.poll());
                } else if (minHeap.size() > maxHeap.size()) {
                    maxHeap.add(minHeap.poll());
                }
            }
            
            // Add the median to the result when the window is fully formed
            if (i >= k - 1) {
                if (maxHeap.size() == minHeap.size()) {
                    result[i - k + 1] = ((double) maxHeap.peek() + minHeap.peek()) / 2;
                } else {
                    result[i - k + 1] = maxHeap.peek();
                }
            }
        }
        
        return result;
    }
}
```