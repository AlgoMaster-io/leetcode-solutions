# 373. [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approach: Min-Heap

### Solution
```java
// Time Complexity: O(k * log(k)), where k is the number of pairs to find
// Space Complexity: O(k), for the heap
import java.util.PriorityQueue;
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        // Min-heap to store pairs along with their sums
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> 
            Integer.compare(a[0] + a[1], b[0] + b[1])
        );

        List<List<Integer>> result = new ArrayList<>();

        // Initialize the heap with pairs (nums1[i], nums2[0]) for all i
        for (int i = 0; i < Math.min(nums1.length, k); i++) {
            minHeap.add(new int[] {nums1[i], nums2[0], 0});
        }

        // Extract the smallest pairs until we have k pairs or the heap is empty
        while (!minHeap.isEmpty() && k > 0) {
            int[] current = minHeap.poll();
            result.add(List.of(current[0], current[1]));
            k--;

            // If there are more pairs with the current nums1[i], add them to the heap
            if (current[2] + 1 < nums2.length) {
                minHeap.add(new int[] {current[0], nums2[current[2] + 1], current[2] + 1});
            }
        }

        return result;
    }
}
```