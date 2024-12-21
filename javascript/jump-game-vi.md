# [Leetcode 1696: Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approaches:
- [Approach 1: Dynamic Programming with Priority Queue](#approach-1-dynamic-programming-with-priority-queue)
- [Approach 2: Dynamic Programming with Deque (Optimal)](#approach-2-dynamic-programming-with-deque-optimal)


### Approach 1: Dynamic Programming with Priority Queue

#### Intuition:
The problem requires us to find a way to maximize the score when jumping from the beginning of the array to the end. We can use dynamic programming to store the maximum score that can be obtained at each index and use a priority queue to efficiently keep track of the best scores within the window of size `k`.

#### Steps:
1. **Initialize a Priority Queue:** Use a max-heap to store the maximum potential score and its index within the window of size `k`.
   
2. **Dynamic Programming Array:** `dp[i]` represents the maximum score that can be acquired to reach index `i`.
   
3. **Iterate through the array:** For each position, fetch the maximum score from the heap that falls within the jump range and update the current dp value.
   
4. **Push the newly calculated dp value and its index into the max-heap.**
  
5. **Return the value at the last index of the dp array which contains the max score to reach the last element.**

#### Code:
```javascript
var maxResult = function(nums, k) {
    let n = nums.length;
    let dp = Array(n).fill(0);
    dp[0] = nums[0];
    
    let heap = new MaxPriorityQueue();
    heap.enqueue(0, nums[0]);  // enqueue(index, value)

    for (let i = 1; i < n; i++) {
        // Remove elements from the top of the heap which are out of jump range
        while (heap.front().element < i - k) {
            heap.dequeue();
        }
        
        // Get the maximum score available within window size 'k'
        dp[i] = nums[i] + heap.front().priority;

        // Store the current index and its calculated dp value into the priority queue
        heap.enqueue(i, dp[i]);
    }
    
    return dp[n - 1];
};
```

#### Complexity:
- **Time Complexity:** O(n log k), because each insertion and deletion in the priority queue takes log k time, and we do this n times.
- **Space Complexity:** O(n), due to the storage of the dp array and priority queue.

### Approach 2: Dynamic Programming with Deque (Optimal)

#### Intuition:
This approach uses a deque to efficiently keep track of the maximum values within the sliding window. By maintaining a monotonically decreasing deque, we can ensure that the front of the deque always contains the maximum score of the window.

#### Steps:
1. **Initialize a Deque:** Use it to store indices of elements. The deque will be maintained in a way that `nums[deque[i]]` is in a descending order.
   
2. **Set the First Element:** The initial score is simply the first number in the array.

3. **Iterate through the Array:** 
   - Remove indices from the back of the deque that have smaller scores than the current `dp[i]`.
   - Remove indices from the front of the deque which are out of the window size `k`.
   - Calculate `dp[i]` using the maximum value at the front of the deque and add the current num to it.
   - Append the current index to the deque

4. **Return the value at the last index of the dp array which contains the max score to reach the last element.**

#### Code:
```javascript
var maxResult = function(nums, k) {
    let n = nums.length;
    let dp = Array(n).fill(0);
    dp[0] = nums[0];
    
    let deque = [];
    deque.push(0);

    for (let i = 1; i < n; i++) {
        // Remove indices that are out of the k-window size
        while (deque.length && deque[0] < i - k) {
            deque.shift();
        }
        
        // Update dp[i] using the max value in the current window
        dp[i] = nums[i] + dp[deque[0]];
        
        // Maintain a monotonically decreasing deque
        while (deque.length && dp[deque[deque.length - 1]] <= dp[i]) {
            deque.pop();
        }
        
        // Push the current index to the deque
        deque.push(i);
    }
    
    return dp[n - 1];
};
```

#### Complexity:
- **Time Complexity:** O(n), because each index is inserted and removed from the deque at most once.
- **Space Complexity:** O(n), due to the storage of the dp array and deque. 

The deque approach optimizes space and time complexity compared to using a priority queue, by efficiently managing the score updates within the specified window size constraints.

