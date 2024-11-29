# Sliding Window Median

## Approach 1: Two Heaps (Max-Heap and Min-Heap)

### Solution
java
```java
// Time Complexity: O(n log k)
// Space Complexity: O(k)
import java.util.Collections;
import java.util.PriorityQueue;

public class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        double[] result = new double[nums.length - k + 1];

        for (int i = 0; i < nums.length; i++) {
            if (maxHeap.isEmpty() || nums[i] <= maxHeap.peek()) {
                maxHeap.add(nums[i]);
            } else {
                minHeap.add(nums[i]);
            }

            balanceHeaps(maxHeap, minHeap);

            if (i >= k - 1) {
                if (maxHeap.size() == minHeap.size()) {
                    result[i - k + 1] = ((double) maxHeap.peek() + minHeap.peek()) / 2;
                } else {
                    result[i - k + 1] = maxHeap.peek();
                }

                int elementToRemove = nums[i - k + 1];
                if (!maxHeap.remove(elementToRemove)) {
                    minHeap.remove(elementToRemove);
                }
                balanceHeaps(maxHeap, minHeap);
            }
        }

        return result;
    }

    private void balanceHeaps(PriorityQueue<Integer> maxHeap, PriorityQueue<Integer> minHeap) {
        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.add(maxHeap.poll());
        } else if (maxHeap.size() < minHeap.size()) {
            maxHeap.add(minHeap.poll());
        }
    }
}
```

### Solution
python
```python
# Time Complexity: O(n log k)
# Space Complexity: O(k)
import heapq

class Solution:
    def medianSlidingWindow(self, nums, k):
        min_heap = []
        max_heap = []
        result = []

        for i in range(len(nums)):
            if not max_heap or nums[i] <= -max_heap[0]:
                heapq.heappush(max_heap, -nums[i])
            else:
                heapq.heappush(min_heap, nums[i])

            self.balance_heaps(max_heap, min_heap)

            if i >= k - 1:
                if len(max_heap) == len(min_heap):
                    result.append((-max_heap[0] + min_heap[0]) / 2)
                else:
                    result.append(-max_heap[0])

                element_to_remove = nums[i - k + 1]
                if element_to_remove <= -max_heap[0]:
                    max_heap.remove(-element_to_remove)
                    heapq.heapify(max_heap)
                else:
                    min_heap.remove(element_to_remove)
                    heapq.heapify(min_heap)
                self.balance_heaps(max_heap, min_heap)

        return result

    def balance_heaps(self, max_heap, min_heap):
        if len(max_heap) > len(min_heap) + 1:
            heapq.heappush(min_heap, -heapq.heappop(max_heap))
        elif len(max_heap) < len(min_heap):
            heapq.heappush(max_heap, -heapq.heappop(min_heap))
```

