## [Leetcode 239: Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

### Approaches:

1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Deque Approach](#optimized-deque-approach)

---

### Brute Force Approach

The brute force approach involves calculating the maximum for each window independently by checking all the elements within the window. This is a straightforward solution but inefficient given the constraints.

#### Intuition:

- We slide a window of size `k` over the array.
- For each starting position of the window, we compute the maximum.
- This involves iterating over all elements in the current window to find the maximum.

#### Time Complexity:
- **O(n*k)**: We have an outer loop running `n-k+1` times and an inner loop running `k` times for each window.

#### Space Complexity:
- **O(1)**: We use no extra data structures except for the output array.

#### Code:

```typescript
function maxSlidingWindow(nums: number[], k: number): number[] {
    // Result array to store the maximums of each window
    const result: number[] = [];
    // Iterate over the array such that each subarray of length `k` is considered
    for (let i = 0; i <= nums.length - k; i++) {
        // Initialize the maximum with the smallest possible number
        let max = -Infinity;
        // Iterate over the window
        for (let j = i; j < i + k; j++) {
            // Update the maximum value
            max = Math.max(max, nums[j]);
        }
        // Add the current maximum to the result
        result.push(max);
    }
    return result;
}
```

---

### Optimized Deque Approach

The optimized approach uses a deque (double-ended queue) to maintain the indices of the useful elements for finding the maximum in the current window. This approach significantly reduces the time complexity.

#### Intuition:

- Keep track of indices in a deque where the elements are in decreasing order (front to back), so the front always contains the index of the maximum element for the current window.
- Remove indices from the front of the deque if they are out of the current window.
- Before adding a new element to the deque, remove all indices from the back where the corresponding elements are less than the current element since they won't be needed anymore.
- The front of the deque always gives the index of the maximum element in the current window.

#### Time Complexity:
- **O(n)**: Each element is pushed and popped from the deque at most once.

#### Space Complexity:
- **O(k)**: The space required for the deque in the worst case.

#### Code:

```typescript
function maxSlidingWindow(nums: number[], k: number): number[] {
    const result: number[] = [];
    const deque: number[] = []; // Will store indices

    for (let i = 0; i < nums.length; i++) {
        // Remove indices that are out of the current window
        if (deque.length > 0 && deque[0] < i - k + 1) {
            deque.shift();
        }

        // Remove indices from the back where the corresponding elements
        // are less than the current element nums[i]
        while (deque.length > 0 && nums[deque[deque.length - 1]] < nums[i]) {
            deque.pop();
        }

        // Add current index at the back of deque
        deque.push(i);

        // If we've processed at least k elements, add the front to the results
        if (i >= k - 1) {
            result.push(nums[deque[0]]);
        }
    }

    return result;
}
```

Feel free to navigate back to the approaches and pick the one that suits your needs given the constraints of your problem!

