# [Leetcode 373: Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Heap Approach](#heap-approach)
- [Optimized Heap Approach using Min-Heap](#optimized-heap-approach-using-min-heap)

## Brute Force Approach

### Intuition:
In the brute force approach, we aim to calculate the sum of every possible pair from two arrays and sort these sums to find the k smallest sums.

### Steps:
1. Iterate through every element in the first array `nums1`.
2. For each element in `nums1`, iterate through every element in the second array `nums2`.
3. Create all possible pairs and store them with their sums.
4. Sort the list of pairs based on the sum.
5. Return the first k pairs.

### Code:
```java
import java.util.*;

public class Solution {
    public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        List<int[]> pairs = new ArrayList<>();
        for (int i : nums1) {
            for (int j : nums2) {
                // Add all possible pairs to the list
                pairs.add(new int[] { i, j });
            }
        }
        // Sort pairs based on the sum
        pairs.sort(Comparator.comparingInt(a -> a[0] + a[1]));
        // Return first k pairs
        return pairs.subList(0, Math.min(k, pairs.size()));
    }
}
```

### Time Complexity:
- O(m * n * log(m * n)), where m and n are the lengths of nums1 and nums2 respectively. Sorting the pair sums is the most time-consuming operation.

### Space Complexity:
- O(m * n), since we need to store all m * n possible pairs.

## Heap Approach

### Intuition:
We can improve our solution by using a max heap to maintain the k smallest pairs found so far while iterating through the array.

### Steps:
1. Use a Max-Heap to keep track of the smallest k sums.
2. Iterate through all pairs of the given arrays.
3. Maintain only k sums in the heap, and pop the largest if the heap size exceeds k.
4. Return the k pairs in the heap.

### Code:
```java
import java.util.*;

public class Solution {
    public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> (b[0] + b[1]) - (a[0] + a[1]));
        for (int i : nums1) {
            for (int j : nums2) {
                int[] pair = new int[] { i, j };
                maxHeap.offer(pair);
                // Ensure only k elements remain in the heap
                if (maxHeap.size() > k) {
                    maxHeap.poll();
                }
            }
        }
        List<int[]> result = new ArrayList<>();
        while (!maxHeap.isEmpty()) {
            result.add(maxHeap.poll());
        }
        Collections.reverse(result);
        return result;
    }
}
```

### Time Complexity:
- O(m * n * log(k)), the log(k) comes from maintaining a heap of size k.

### Space Complexity:
- O(k), since the heap only stores k pairs at any time.

## Optimized Heap Approach using Min-Heap

### Intuition:
Instead of using a max heap, we can use a min heap (priority queue) to efficiently get the next smallest sum. 

### Steps:
1. Initiate a Min-Heap to store potential pairs, starting with the smallest possible pairs (first elements of `nums1` with first element of `nums2`).
2. Extract the smallest pair from the heap.
3. Push the next possible pairs incrementally from `nums1` and `nums2`.
4. Repeat until we find k pairs.

### Code:
```java
import java.util.*;

public class Solution {
    public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        List<int[]> result = new ArrayList<>();
        if (nums1.length == 0 || nums2.length == 0 || k == 0) {
            return result;
        }
        
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> (a[0] + a[1]) - (b[0] + b[1]));
        
        // Initialize the min heap with the first element in nums2 combined with every element in nums1
        for (int i = 0; i < nums1.length && i < k; i++) {
            minHeap.offer(new int[] { nums1[i], nums2[0], 0 });
        }
        
        // Extract the smallest pairs
        while (k-- > 0 && !minHeap.isEmpty()) {
            int[] curr = minHeap.poll();
            result.add(new int[] { curr[0], curr[1] });

            // If there is a next pair in the row, add it to the heap
            if (curr[2] == nums2.length - 1) {
                continue;
            }
            minHeap.offer(new int[] { curr[0], nums2[curr[2] + 1], curr[2] + 1 });
        }
        return result;
    }
}
```

### Time Complexity:
- O(k * log(min(m, n, k))), where you might need to retrieve up to k pairs, and each operation in the heap is logarithmic to its size.

### Space Complexity:
- O(min(m, n, k)), due to the space needed for the priority queue storing potential pairs.

