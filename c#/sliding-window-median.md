# [Leetcode 480: Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Using MultiSet for Median Calculation](#multiset-approach)
3. [Using Two Heaps (Optimal)](#two-heaps-approach)

---

### Brute Force Approach

#### Intuition:
The simplest way to solve this problem is to iterate through `nums`, and for each possible window of `k` elements, extract the window, sort it, and then find its median directly.

#### Steps:
1. Iterate through `nums` with a window sliding from the start to the end minus `k`.
2. In each iteration, extract the current window.
3. Sort the window.
4. Calculate the median based on whether `k` is even or odd.
5. Store the median and move to the next window.

#### Time Complexity:
- **Time**: `O(n*k*log(k))` due to sorting each window of size `k`.
- **Space**: `O(k)` for storing each window.

#### C# Code:
```csharp
public double[] MedianSlidingWindow(int[] nums, int k) {
    int n = nums.Length;
    double[] medians = new double[n - k + 1];

    for (int i = 0; i <= n - k; i++) {
        int[] window = new int[k];
        // Extract the window
        Array.Copy(nums, i, window, 0, k);
        // Sort the window to calculate median
        Array.Sort(window);
        // If k is even, the median is the average of the two middle numbers
        if (k % 2 == 0) {
            medians[i] = ((double)window[k / 2 - 1] + window[k / 2]) / 2;
        } else {
            medians[i] = window[k / 2];
        }
    }
    return medians;
}
```

---

### MultiSet Approach

#### Intuition:
Using a structure akin to a multiset can help in managing the insertion, deletion, and median extraction faster than repeatedly sorting the window. 

#### Steps:
1. Maintain a sorted data structure to keep track of the k elements.
2. Use efficient insert and remove operations to slide the window.
3. Retrieve the median of the structure in `O(1)` time.

#### Time Complexity:
- **Time**: `O(n*k)` using a naive binary search tree for insertion and removal.
- **Space**: `O(k)` for the data structure.

This approach is theoretical in C# since native support for efficient `multiset` operations like C++ STL multiset is not directly available. One can use `SortedList` or `SortedDictionary` and manage counts to simulate.

---

### Two Heaps Approach (Optimal)

#### Intuition:
The most optimal way to find the median of a k-sized window is to use two heaps. One heap (a max-heap) for the lower half of numbers and one heap (a min-heap) for the upper half. The median is readily available from the tops of these heaps.

#### Steps:
1. Maintain a max-heap for the lower half of the numbers.
2. Maintain a min-heap for the upper half of the numbers.
3. Ensure heaps are balanced such that max-heap can have at most one more element than the min-heap.
4. For each move of the window, manage these heaps to add the new element and remove the old element.
5. Collect the median accordingly:
    - If heaps are balanced, the median is the average of the two tops.
    - Otherwise, it's the top of the max-heap.

#### Time Complexity:
- **Time**: `O(n*log(k))` for managing heaps of size `k` efficiently.
- **Space**: `O(k)` for storing numbers in heaps.

#### C# Code:
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public double[] MedianSlidingWindow(int[] nums, int k) {
        List<double> medians = new List<double>();
        PriorityQueue<int, int> low = new PriorityQueue<int, int>(Comparer<int>.Create((x, y) => y.CompareTo(x))); // max heap
        PriorityQueue<int, int> high = new PriorityQueue<int, int>(); // min heap
        
        for (int i = 0; i < nums.Length; i++) {
            // Add new number to appropriate heap
            if (low.Count == 0 || nums[i] <= low.Peek()) {
                low.Enqueue(nums[i], nums[i]);
            } else {
                high.Enqueue(nums[i], nums[i]);
            }

            // Remove element outside of window
            if (i >= k) {
                if (nums[i - k] <= low.Peek()) {
                    Remove(low, nums[i - k]);
                } else {
                    Remove(high, nums[i - k]);
                }
            }

            // Balance heaps
            BalanceHeaps(low, high);

            // Get median
            if (i >= k - 1) {
                if (low.Count > high.Count) {
                    medians.Add((double)low.Peek());
                } else {
                    medians.Add(((double)low.Peek() + high.Peek()) / 2.0);
                }
            }
        }

        return medians.ToArray();
    }

    private void BalanceHeaps(PriorityQueue<int, int> low, PriorityQueue<int, int> high) {
        if (low.Count > high.Count + 1) {
            high.Enqueue(low.Dequeue(), low.Peek());
        } else if (low.Count < high.Count) {
            low.Enqueue(high.Dequeue(), high.Peek());
        }
    }
    
    private void Remove(PriorityQueue<int, int> heap, int value) {
        List<int> temp = new List<int>();
        while (heap.Count > 0) {
            int cur = heap.Dequeue();
            if (cur != value) {
                temp.Add(cur);
            } else {
                break;
            }
        }
        foreach (var item in temp) {
            heap.Enqueue(item, item);
        }
    }
}
```

The implementation uses `PriorityQueue` for the heap structure available in C#. However, in practical usage, one might need to create a utility method to directly mimic the C++ heap functionality. The explanation, the balancing logic, and median extraction are crucial steps in achieving this optimal running time.

