# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approach 1: Using Deque (Optimal Solution)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(k)
using System.Collections.Generic;

public class Solution {
    public int[] MaxSlidingWindow(int[] nums, int k) {
        int n = nums.Length;
        int[] result = new int[n - k + 1];
        Deque<int> deque = new Deque<int>(); // Stores indices of elements

        for (int i = 0; i < n; i++) {
            // Remove indices out of the current window
            if (deque.Count > 0 && deque.PeekFirst() < i - k + 1) {
                deque.RemoveFirst();
            }

            // Remove elements smaller than the current element from the back
            while (deque.Count > 0 && nums[deque.PeekLast()] < nums[i]) {
                deque.RemoveLast();
            }

            deque.AddLast(i); // Add the current element's index to the deque

            // Add the maximum element of the current window to the result
            if (i >= k - 1) {
                result[i - k + 1] = nums[deque.PeekFirst()];
            }
        }

        return result;
    }
}
```

## Approach 2: Using Priority Queue (Heap-Based)

### Solution
csharp
```csharp
// Time Complexity: O(n log k)
// Space Complexity: O(k)
using System.Collections.Generic;

public class Solution {
    public int[] MaxSlidingWindow(int[] nums, int k) {
        int n = nums.Length;
        int[] result = new int[n - k + 1];
        PriorityQueue<(int Value, int Index), int> maxHeap = new PriorityQueue<(int Value, int Index), int>(
            Comparer<int>.Create((a, b) => b - a)); // Max-heap: {value, index}

        for (int i = 0; i < n; i++) {
            // Add the current element to the heap
            maxHeap.Enqueue((nums[i], i), nums[i]);

            // Remove elements that are out of the current window
            while (maxHeap.Peek().Index < i - k + 1) {
                maxHeap.Dequeue();
            }

            // Add the maximum element of the current window to the result
            if (i >= k - 1) {
                result[i - k + 1] = maxHeap.Peek().Value;
            }
        }

        return result;
    }
}
```

