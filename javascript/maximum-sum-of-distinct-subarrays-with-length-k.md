[LeetCode Problem 2461: Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

### Approaches
1. [Brute Force](#brute-force)
2. [Sliding Window](#sliding-window)

---

### Brute Force

#### Intuition
The idea behind the brute force approach is to explore all possible subarrays of length `k` and check if they are distinct by verifying that all elements in the subarray are unique. For each valid subarray, we calculate the sum and keep track of the maximum sum found.

#### Steps
1. Iterate over each possible subarray starting index.
2. For each subarray, use another loop to check if the subarray is distinct.
3. If distinct, sum up the subarray and update the maximum sum encountered.
4. Return the maximum sum after checking all subarrays.

#### Code

```javascript
function maxSumSubarray(arr, k) {
  let maxSum = 0;
  
  // Iterate over starting index of each subarray of length k
  for (let i = 0; i <= arr.length - k; i++) {
    let subarray = arr.slice(i, i + k);
    
    // Check for distinct elements
    let uniqueSet = new Set(subarray);
    
    // If distinct, consider the sum
    if (uniqueSet.size === k) {
      let currentSum = subarray.reduce((acc, val) => acc + val, 0);
      maxSum = Math.max(maxSum, currentSum);
    }
  }
  
  return maxSum;
}
```

#### Complexity
- **Time Complexity**: O(n*k), where n is the length of the array and k is the subarray length.
- **Space Complexity**: O(k), for storing each subarray and the set.

---

### Sliding Window

#### Intuition
The sliding window technique optimizes the brute force by maintaining a window of size `k` that slides through the array while keeping track of the sum and uniqueness of the elements efficiently using a HashSet.

#### Steps
1. Use a sliding window technique with two pointers.
2. Maintain a HashSet to track unique elements in the current window and a variable to store the current window sum.
3. If the element to be added in the window is distinct, add it to the sum and set.
4. If an element needs to be discarded (when moving the window), subtract it from the sum and remove it from the set.
5. Keep track of the maximum sum during the window traversal.

#### Code

```javascript
function maxSumSubarray(arr, k) {
  let maxSum = 0;
  let currentSum = 0;
  let uniqueSet = new Set();
  
  for (let i = 0, j = 0; j < arr.length; j++) {
    while (uniqueSet.has(arr[j])) {
      // Remove element from the left of the window
      uniqueSet.delete(arr[i]);
      currentSum -= arr[i];
      i++;
    }
    
    // Add new element to the window
    uniqueSet.add(arr[j]);
    currentSum += arr[j];
    
    // If window size equals k, update maxSum
    if (j - i + 1 === k) {
      maxSum = Math.max(maxSum, currentSum);
      // Prepare window for next element by removing leftmost element
      uniqueSet.delete(arr[i]);
      currentSum -= arr[i];
      i++;
    }
  }
  
  return maxSum;
}
```

#### Complexity
- **Time Complexity**: O(n), where n is the length of the array as each element is added and removed from the set at most once.
- **Space Complexity**: O(k), for storing unique elements in the set.

