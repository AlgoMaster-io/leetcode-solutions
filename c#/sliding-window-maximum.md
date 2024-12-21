# [LeetCode 239: Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approaches:
- [Naive Approach: Brute Force](#naive-approach)
- [Optimized Approach: Deque (Double-Ended Queue)](#optimized-approach)

### Naive Approach: Brute Force

#### Intuition
This is the most straightforward way to solve the problem. For each possible window within the array, calculate the maximum and store it. Although simple, it's inefficient because it has to iterate over the same elements repeatedly to find the maximum in every window.

#### Solution

```csharp
public int[] MaxSlidingWindow(int[] nums, int k) {
    if (nums == null || nums.Length == 0) return new int[0];
    int n = nums.Length;
    int[] result = new int[n - k + 1];
    
    // Iterate over each possible window start index
    for (int i = 0; i <= n - k; i++) {
        // Assume the first element of the window is the maximum
        int max = nums[i];
        
        // Check for a larger element in the current window
        for (int j = 1; j < k; j++) {
            if (nums[i + j] > max) {
                max = nums[i + j];
            }
        }
        
        // Store the maximum in the result array
        result[i] = max;
    }
    
    return result;
}
```

#### Time Complexity
- O(n * k), where n is the number of elements in the array. We check k elements for each window position.

#### Space Complexity
- O(n - k + 1), due to the storage of the result.

---

### Optimized Approach: Deque (Double-Ended Queue)

#### Intuition
Using a deque is an optimal way to solve this problem as it allows maintaining the maximum for the current window efficiently. The deque will store indexes of the array elements. It ensures that the elements in the deque are always in descending order. The element at the front of the deque is always the maximum of the current window. As the window slides, we add new elements to the deque and remove elements that are not within the current window.

#### Solution

```csharp
public int[] MaxSlidingWindow(int[] nums, int k) {
    if (nums == null || nums.Length == 0) return new int[0];
    int n = nums.Length;
    int[] result = new int[n - k + 1];
    LinkedList<int> deque = new LinkedList<int>();

    for (int i = 0; i < n; i++) {
        // Remove elements not within the sliding window
        if (deque.Count > 0 && deque.First.Value < i - k + 1) {
            deque.RemoveFirst();
        }

        // Remove elements from the back that are smaller than the current element
        while (deque.Count > 0 && nums[deque.Last.Value] < nums[i]) {
            deque.RemoveLast();
        }

        // Add the current element's index to the deque
        deque.AddLast(i);

        // Storing result for each complete sliding window
        if (i >= k - 1) {
            result[i - k + 1] = nums[deque.First.Value];
        }
    }

    return result;
}
```

#### Time Complexity
- O(n), where n is the number of elements in the array. Each element is added and removed from the deque once.

#### Space Complexity
- O(k), for the deque that keeps indices of potential maximums within the window.

