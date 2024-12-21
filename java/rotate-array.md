# [Leetcode 189: Rotate Array](https://leetcode.com/problems/rotate-array/)

## Approaches

- [Approach 1: Brute Force with Rotation Simulation](#approach-1-brute-force-with-rotation-simulation)
- [Approach 2: Extra Array for New Order](#approach-2-extra-array-for-new-order)
- [Approach 3: Reverse the Array](#approach-3-reverse-the-array)

---

## Approach 1: Brute Force with Rotation Simulation

### Intuition
The brute force approach involves rotating the array elements one step at a time. For each rotation, we move all the elements to their next position, simulating the rotation `k` times. Though simple, this solution is inefficient due to repeated movements of elements.

### Steps
1. Perform `k` individual rotations.
2. For each rotation, move every element one position forward, and wrap the last element to the front.

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        // Each rotation moves all elements 1 step to the right
        k = k % n; // Handle cases where k >= n
        for (int i = 0; i < k; i++) {
            // Store the last element
            int previous = nums[n - 1];
            // Shift all elements right
            for (int j = n - 1; j > 0; j--) {
                nums[j] = nums[j - 1];
            }
            // Place the stored element at the first position
            nums[0] = previous;
        }
    }
}
```

**Time Complexity:** O(n * k) - Performing `k` rotations, each requiring O(n) time.

**Space Complexity:** O(1) - No additional space used apart from input array.

---

## Approach 2: Extra Array for New Order

### Intuition
By using an additional array, you can directly place each element in its rotated position. This avoids costly movement within the original array but requires additional space for storing the results.

### Steps
1. Create a new array to hold rotated elements.
2. Calculate and place each element in its final position.
3. Copy the result back into the original array.

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        int[] rotated = new int[n];
        k = k % n; // Handle cases where k >= n
        
        // Place elements in the new positions
        for (int i = 0; i < n; i++) {
            rotated[(i + k) % n] = nums[i];
        }
        
        // Copy the content of rotated array to the original array
        for (int i = 0; i < n; i++) {
            nums[i] = rotated[i];
        }
    }
}
```

**Time Complexity:** O(n) - Each element is processed a constant number of times.

**Space Complexity:** O(n) - Additional array to hold rotated order.

---

## Approach 3: Reverse the Array

### Intuition
The optimal solution utilizes array reversing. The underlying idea is that rotating an array is equivalent to reversing parts of the array. Specifically:
1. Reverse the entire array.
2. Reverse the first `k` elements.
3. Reverse the elements from `k` to the end.

### Steps
1. Reverse the whole array.
2. Reverse the first `k` elements.
3. Reverse the remaining `n-k` elements.

### Explanation
- Reversing the array repositions elements such that their relative distance with respect to rotation is preserved.
- Reversing segments then readjusts their final positions.

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n; // Handle cases where k >= n
        
        // Step 1: Reverse the whole array
        reverse(nums, 0, n - 1);
        // Step 2: Reverse the first k elements
        reverse(nums, 0, k - 1);
        // Step 3: Reverse the remaining elements
        reverse(nums, k, n - 1);
    }

    private void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```

**Time Complexity:** O(n) - Each reverse operation is O(n), and we perform a constant number of them.

**Space Complexity:** O(1) - Reversal done in-place without extra space.

