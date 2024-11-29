# [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approach: Two Heaps

### Solution

java
```java
// Time Complexity: O(n log k), where n is the number of elements and k is the size of the sliding window
// Space Complexity: O(k), where k is the size of the sliding window

import java.util.*;

public class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        // maxHeap for the first half
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        // minHeap for the second half
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        List<Double> result = new ArrayList<>();

        for (int i = 0; i < nums.length; i++) {
            if (maxHeap.size() <= minHeap.size()) {
                minHeap.offer(nums[i]);
                maxHeap.offer(minHeap.poll());
            } else {
                maxHeap.offer(nums[i]);
                minHeap.offer(maxHeap.poll());
            }

            if (maxHeap.size() + minHeap.size() == k) {
                // Compute the median
                if (maxHeap.size() == minHeap.size()) {
                    result.add(((double) maxHeap.peek() + (double) minHeap.peek()) / 2);
                } else {
                    result.add((double) maxHeap.peek());
                }

                // Remove the element going out of the window
                int elementToRemove = nums[i - k + 1];
                if (!maxHeap.remove(elementToRemove)) {
                    minHeap.remove(elementToRemove);
                }
            }
        }

        // Convert result to defined array
        double[] res = new double[result.size()];
        for (int i = 0; i < result.size(); i++) {
            res[i] = result.get(i);
        }
        return res;
    }
}
```

typescript
```typescript
// Time Complexity: O(n log k), where n is the number of elements and k is the size of the sliding window
// Space Complexity: O(k), where k is the size of the sliding window

class Solution {
    medianSlidingWindow(nums: number[], k: number): number[] {
        const maxHeap = new MaxPriorityQueue<number>({ priority: x => x });
        const minHeap = new MinPriorityQueue<number>({ priority: x => -x });
        const result: number[] = [];

        for (let i = 0; i < nums.length; i++) {
            if (maxHeap.size() <= minHeap.size()) {
                minHeap.enqueue(nums[i]);
                maxHeap.enqueue(minHeap.dequeue().element);
            } else {
                maxHeap.enqueue(nums[i]);
                minHeap.enqueue(maxHeap.dequeue().element);
            }

            if (maxHeap.size() + minHeap.size() === k) {
                // Compute the median
                if (maxHeap.size() === minHeap.size()) {
                    result.push((maxHeap.front().element + minHeap.front().element) / 2);
                } else {
                    result.push(maxHeap.front().element);
                }

                // Remove the element going out of the window
                const elementToRemove = nums[i - k + 1];
                if (!maxHeap.remove(elementToRemove)) {
                    minHeap.remove(elementToRemove);
                }
            }
        }

        return result;
    }
}
```

*Note: The above TypeScript code uses a priority queue implementation that is available in npm package @datastructures-js/priority-queue. You will need to install this package to run the TypeScript solution.*

