# [480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approaches
- [Approach 1: Brute Force Using Sorting](#approach-1-brute-force-using-sorting)
- [Approach 2: Maintaining Two Heaps (Optimal)](#approach-2-maintaining-two-heaps-optimal)

---

### Approach 1: Brute Force Using Sorting

#### Intuition:
The brute force approach involves recalculating the median every time the sliding window moves. For each new window, we sort the window elements and directly compute the median. This approach is straightforward but inefficient, as sorting each time takes considerable time.

#### Steps:
1. Iterate through the `nums` array, from the start to the end minus the window size `k`.
2. For each position, get the subarray of size `k` and sort it.
3. Calculate the median:
   - If `k` is odd, the median is the middle element of the sorted window.
   - If `k` is even, the median is the average of the two middle elements.
4. Store the median in the result.

#### Time Complexity:
- Sorting each window takes \(O(k \log k)\), and there are \(n-k+1\) windows.
- Total time complexity is \(O((n-k+1) \times k \log k)\).

#### Space Complexity:
- \(O(k)\), as we store the subarray for sorting.

```java
import java.util.*;

public class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        double[] medians = new double[n - k + 1];
        
        for (int i = 0; i <= n - k; i++) {
            int[] window = new int[k];
            System.arraycopy(nums, i, window, 0, k);
            Arrays.sort(window);
            if (k % 2 == 0) {
                medians[i] = ((double)window[k / 2 - 1] + window[k / 2]) / 2;
            } else {
                medians[i] = window[k / 2];
            }
        }
        
        return medians;
    }
}
```

---

### Approach 2: Maintaining Two Heaps (Optimal)

#### Intuition:
We can use two heaps to maintain the order of the sliding window:
1. A max-heap to store the smaller half of the numbers.
2. A min-heap to store the larger half of the numbers.

By always maintaining balance between the two heaps, the median can be derived efficiently:
- If k is odd, the max-heap will give the median.
- If k is even, the median is the average of the roots of both heaps.

#### Steps:
1. Use a `PriorityQueue` for both heaps.
2. Process the elements of the array: add new numbers, remove the old numbers going out of the window.
3. Ensure the balance between the two heaps every time a insertion or removal is done.
4. Calculate the median using the top elements of the heaps.

#### Time Complexity:
- Each operation of insertion/removal/balance takes \(O(\log k)\).
- For each window, there are approximately constant number of operations.
- Overall time complexity is \(O(n \log k)\).

#### Space Complexity:
- \(O(k)\), space used by both heaps together.

```java
import java.util.*;

public class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        double[] medians = new double[n - k + 1];
        
        // Max-heap to maintain the smaller half
        PriorityQueue<Integer> small = new PriorityQueue<>(Collections.reverseOrder());
        // Min-heap to maintain the larger half
        PriorityQueue<Integer> large = new PriorityQueue<>();
        
        for (int i = 0; i < n; i++) {
            if (small.isEmpty() || nums[i] <= small.peek()) {
                small.offer(nums[i]);
            } else {
                large.offer(nums[i]);
            }
            
            if (i >= k) {
                if (small.contains(nums[i - k])) {
                    small.remove(nums[i - k]);
                } else {
                    large.remove(nums[i - k]);
                }
            }
            
            // Balance the heaps
            while (small.size() > large.size() + 1) {
                large.offer(small.poll());
            }
            while (large.size() > small.size()) {
                small.offer(large.poll());
            }
            
            // Get the median
            if (i >= k - 1) {
                if (k % 2 == 0) {
                    medians[i - k + 1] = ((double)small.peek() + large.peek()) / 2;
                } else {
                    medians[i - k + 1] = small.peek();
                }
            }
        }
        
        return medians;
    }
}
```

The second approach is the most optimal and commonly used in practical applications. It efficiently processes each sliding window by maintaining a balance between two heaps, allowing for quick access to the median.

