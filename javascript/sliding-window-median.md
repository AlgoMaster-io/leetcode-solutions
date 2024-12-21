# [Leetcode 480: Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Two Heaps Approach](#two-heaps-approach)

---

### Brute Force Approach

The naive approach is straightforward but inefficient. For each new window slide, we compute the median by sorting the subarray that falls within the window. This requires handling window updates explicitly by removing the element that slides out and adding the new element that comes in.

**Intuition**:
- Iterate over the array with a sliding window.
- For each window, extract the subarray and sort it.
- Calculate the median from the sorted subarray.
- Since sorting is O(k log k) where k is the window size, it becomes inefficient for large k or total input size n, as each window slide involves sorting.

**Steps**:
1. Initialize an empty array `result` to store medians.
2. Iterate over the array such that we can extract each sliding window.
3. For each window,:
   - Sort the window elements.
   - Compute and store the median:
     - If odd number of elements, pick the middle one.
     - If even, average the two middlemost elements.
4. Return the `result` array.

```javascript
function medianSlidingWindow(nums, k) {
  const result = [];

  for (let i = 0; i <= nums.length - k; i++) {
    const window = nums.slice(i, i + k).sort((a, b) => a - b);

    if (k % 2 === 0) {
      // Even length, median is the average of middle two
      result.push((window[k / 2 - 1] + window[k / 2]) / 2);
    } else {
      // Odd length, median is the middle one
      result.push(window[Math.floor(k / 2)]);
    }
  }

  return result;
}

// Time Complexity: O(n * k log k) where n is the length of nums and k is the window size.
// Space Complexity: O(k) as each window is stored for computation.
```

### Two Heaps Approach

This approach involves maintaining two heaps: a max-heap for the lower half of numbers in the window and a min-heap for the upper half. This allows for faster median calculations than sorting, as heaps provide efficient insertion, deletion, and retrieval.

**Intuition**:
- Use two heaps to balance and maintain the lower and upper halves of the sliding window.
- Ensure the max-heap (`lower`) contains the smaller half, and the min-heap (`upper`) contains the larger.
- The median is either the largest of the lower half (size k/2) or average of largest lower and smallest upper (size k/2 and k/2 + 1).
- When sliding the window, maintain heap balances by adding/removing elements as necessary.

**Steps**:
1. Initialize two heaps: one max-heap (`lower`) and one min-heap (`upper`).
2. For each element in nums:
   - Add it to lower or upper based on comparison with the root of the heaps.
   - Rebalance the heaps to ensure max-heap has either equal elements or one more than min-heap.
   - If there are enough elements to form the first complete window:
     - Compute and store the median from the heaps.
     - Remove the element that goes out of the window.
     - Rebalance as necessary.
3. Extract and return the medians from all windows.

```javascript
function medianSlidingWindow(nums, k) {
  const lower = new MaxHeap(); // Max heap for lower half
  const upper = new MinHeap(); // Min heap for upper half
  const result = [];

  for (let i = 0; i < nums.length; i++) {
    lower.add(nums[i]);  // Add to max-heap
    upper.add(lower.poll()); // Balance by moving largest of lower to upper

    if (lower.size() < upper.size()) {
      lower.add(upper.poll()); // Ensure lower has equal or more elements
    }

    if (i >= k - 1) {  // Window is fully formed
      // Calculate median
      if (k % 2 === 0) {
        result.push((lower.peek() + upper.peek()) / 2);
      } else {
        result.push(lower.peek());
      }

      // Remove the oldest element
      if (!lower.remove(nums[i - k + 1])) {
        upper.remove(nums[i - k + 1]);
      }
    }
  }
  
  return result;
}

// Time Complexity: O(n log k) for n elements, adjusting the heaps takes logarithmic time.
// Space Complexity: O(k) due to storing elements in the heaps.
```

**Note**: Implementing `MaxHeap` and `MinHeap` is crucial for this approach, typically done using JavaScript arrays with appropriate comparator functions for maintaining heap properties efficiently.

