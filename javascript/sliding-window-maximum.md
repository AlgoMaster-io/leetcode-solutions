# [Leetcode 239: Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Navigation
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Max Heap (Priority Queue)](#approach-2-max-heap-priority-queue)
- [Approach 3: Deque (Optimal)](#approach-3-deque-optimal)

---

## Approach 1: Brute Force

### Intuition
The simplest way to solve this problem is by iterating through each possible window of size `k` and finding the maximum element in that window. Although straightforward, this approach is inefficient for large arrays as it repeatedly evaluates the maximum for overlapping sections.

### Algorithm
1. Initialize an array to store the maximums.
2. Loop through the array starting from index `0` to `n-k`, where `n` is the length of the input array.
3. For each window, iterate from the current index to `k` elements ahead and find the maximum.
4. Append the maximum to the result array.
5. Return the result array.

### Code
```javascript
var maxSlidingWindow = function(nums, k) {
    const result = [];
    // Loop over each window starting point
    for (let i = 0; i <= nums.length - k; i++) {
        let max = -Infinity;
        // Find the maximum in the current window
        for (let j = i; j < i + k; j++) {
            max = Math.max(max, nums[j]);
        }
        result.push(max);
    }
    return result;
};
```

### Time Complexity
- O(n*k): For each of the `n-k+1` windows, we find the maximum in O(k) time.

### Space Complexity
- O(n-k+1): Result array to store the maximums of all windows.

---

## Approach 2: Max Heap (Priority Queue)

### Intuition
A max heap can be used to maintain the maximum value efficiently by keeping the maximum at the root. However, managing the size of the heap for each window is crucial in terms of complexity.

### Algorithm
1. Utilize a max heap (priority queue) to keep track of elements in the current window.
2. For each element, add it to the heap.
3. Maintain only elements from the current window in the heap.
4. The max heap’s top element will be the maximum for the current window.
5. Remove elements from the heap that are out of the current window range.

### Code
```javascript
class MaxHeap {
    constructor() {
        this.data = [];
    }

    push(val, index) {
        this.data.push({val, index});
        this.data.sort((a, b) => b.val - a.val);
    }

    remove(index) {
        this.data = this.data.filter((element) => element.index !== index);
    }

    max() {
        return this.data[0].val;
    }
}

var maxSlidingWindow = function(nums, k) {
    const result = [];
    const heap = new MaxHeap();    

    for (let i = 0; i < nums.length; i++) {
        // Add the current element into the heap
        heap.push(nums[i], i);

        // Remove the element that is out of the current window
        if (i >= k) {
            heap.remove(i - k);
        }

        // Record the max value of the current window
        if (i >= k - 1) {
            result.push(heap.max());
        }
    }

    return result;
};
```

### Time Complexity
- O(n*log(k)): Each insertion and removal in the heap takes logarithmic time relative to the heap's size which leads to n operations overall.

### Space Complexity
- O(k): MaxHeap stores only elements in the current window.

---

## Approach 3: Deque (Optimal)

### Intuition
Using a double-ended queue (deque), we can keep track of the indices of potential maximum elements in the current window. The deque is maintained such that the indices corresponding to potential maximums are in decreasing order. This approach efficiently finds the maximum for each window in linear time.

### Algorithm
1. Initialize a deque to store indices.
2. Iterate through each element in nums.
3. Remove elements from the deque's front if they are out of the current window.
4. Remove elements from the deque's back if they are smaller than the current element since they are not needed.
5. Add the current element's index to the deque.
6. Append the element at the deque's front to the result array when the first valid max is available (starting from window size).

### Code
```javascript
var maxSlidingWindow = function(nums, k) {
    const result = [];
    const deque = [];
    
    for (let i = 0; i < nums.length; i++) {
        // Remove elements not within the sliding window
        if (deque.length && deque[0] < i - k + 1) {
            deque.shift();
        }
        
        // Remove elements smaller than the current element from the back of the deque
        while (deque.length && nums[deque[deque.length - 1]] <= nums[i]) {
            deque.pop();
        }
        
        // Add current element's index to the back of the deque
        deque.push(i);
        
        // Append the largest element in the current window to the result
        if (i >= k - 1) {
            result.push(nums[deque[0]]);
        }
    }
    
    return result;
};
```

### Time Complexity
- O(n): Each element is added and removed from the deque at most once.

### Space Complexity
- O(k): In the worst case, the deque will store all elements in a window.

With this approach, we achieve the optimal solution to the problem with linear time complexity.

