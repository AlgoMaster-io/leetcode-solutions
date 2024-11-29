# 480. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approach 1: Two Heaps

### Solution
java
```java
// Time Complexity: O(n log k)
// Space Complexity: O(k)
import java.util.Collections;
import java.util.PriorityQueue;

public class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length < k) {
            return new double[0];
        }

        // Max heap for the first half
        PriorityQueue<Integer> left = new PriorityQueue<>(Collections.reverseOrder());
        // Min heap for the second half
        PriorityQueue<Integer> right = new PriorityQueue<>();

        // Result array
        double[] medians = new double[nums.length - k + 1];

        for (int i = 0; i < nums.length; i++) {
            if (i >= k) {
                int toRemove = nums[i - k];
                if (!left.remove(toRemove)) {
                    right.remove(toRemove);
                }
                balance(left, right);
            }

            // Add new element
            if (left.isEmpty() || nums[i] <= left.peek()) {
                left.add(nums[i]);
            } else {
                right.add(nums[i]);
            }

            // Balance the two heaps
            balance(left, right);

            // Calculate median
            if (i >= k - 1) {
                medians[i - k + 1] = getMedian(left, right);
            }
        }

        return medians;
    }

    private void balance(PriorityQueue<Integer> left, PriorityQueue<Integer> right) {
        if (left.size() > right.size() + 1) {
            right.add(left.poll());
        } else if (right.size() > left.size()) {
            left.add(right.poll());
        }
    }

    private double getMedian(PriorityQueue<Integer> left, PriorityQueue<Integer> right) {
        if (left.size() > right.size()) {
            return left.peek();
        } else {
            return ((double) left.peek() + right.peek()) / 2.0;
        }
    }
}
```

go
```go
// Time Complexity: O(n log k)
// Space Complexity: O(k)
package main

import (
    "container/heap"
)

type maxHeap []int
type minHeap []int

func (h maxHeap) Len() int           { return len(h) }
func (h minHeap) Len() int           { return len(h) }
func (h maxHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h minHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h maxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h minHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *maxHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *minHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *maxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func (h *minHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func medianSlidingWindow(nums []int, k int) []float64 {
    if nums == nil || len(nums) < k {
        return []float64{}
    }

    left := &maxHeap{}
    right := &minHeap{}

    result := make([]float64, 0, len(nums)-k+1)

    for i := 0; i < len(nums); i++ {
        if i >= k {
            toRemove := nums[i-k]
            if contains(*left, toRemove) {
                remove(left, toRemove)
            } else {
                remove(right, toRemove)
            }
            balance(left, right)
        }

        if left.Len() == 0 || nums[i] <= (*left)[0] {
            heap.Push(left, nums[i])
        } else {
            heap.Push(right, nums[i])
        }

        balance(left, right)

        if i >= k-1 {
            result = append(result, getMedian(left, right))
        }
    }

    return result
}

func balance(left *maxHeap, right *minHeap) {
    if left.Len() > right.Len()+1 {
        heap.Push(right, heap.Pop(left))
    } else if right.Len() > left.Len() {
        heap.Push(left, heap.Pop(right))
    }
}

func getMedian(left *maxHeap, right *minHeap) float64 {
    if left.Len() > right.Len() {
        return float64((*left)[0])
    }
    return (float64((*left)[0]) + float64((*right)[0])) / 2.0
}

func contains(h maxHeap, val int) bool {
    for _, v := range h {
        if v == val {
            return true
        }
    }
    return false
}

func containsMin(h minHeap, val int) bool {
    for _, v := range h {
        if v == val {
            return true
        }
    }
    return false
}

func remove(h *maxHeap, val int) {
    idx := -1
    for i, v := range *h {
        if v == val {
            idx = i
            break
        }
    }
    if idx >= 0 {
        heap.Remove(h, idx)
    }
}

func removeMin(h *minHeap, val int) {
    idx := -1
    for i, v := range *h {
        if v == val {
            idx = i
            break
        }
    }
    if idx >= 0 {
        heap.Remove(h, idx)
    }
}
```

