# [Leetcode 42: Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

## Approaches

- [Brute Force](#brute-force)
- [Dynamic Programming with Precomputation](#dynamic-programming-with-precomputation)
- [Two Pointers](#two-pointers)

---

### Brute Force

The brute force approach involves calculating the trapped water at each element by finding the maximum height on the left and right for that element.

**Intuition:**

1. To determine the water at a position `i`, we need to find the minimum of the maximum height on the left and right of the element.
2. The trapped water at `i` will be the difference between this minimum value and the height at `i`.

**Algorithm:**

1. Initialize `water` to `0` to store the total trapped rainwater.
2. For each element (except the first and last), calculate:
   - `left_max`: The maximum height from the start to the current element.
   - `right_max`: The maximum height from the current element to the end.
   - Calculate the water trapped at that element as `min(left_max, right_max) - height[i]`.
3. Add the trapped water for each element to `water`.

```typescript
function trap(height: number[]): number {
    let water = 0;
    const n = height.length;

    for (let i = 1; i < n - 1; i++) {
        // Find the max height to the left of current position
        let left_max = 0;
        for (let j = 0; j <= i; j++) {
            left_max = Math.max(left_max, height[j]);
        }

        // Find the max height to the right of current position
        let right_max = 0;
        for (let j = i; j < n; j++) {
            right_max = Math.max(right_max, height[j]);
        }

        // Calculate trapped water at position i
        water += Math.min(left_max, right_max) - height[i];
    }

    return water;
}
```

**Time Complexity:** \(O(n^2)\)  
**Space Complexity:** \(O(1)\)

---

### Dynamic Programming with Precomputation

We can optimize the brute force approach by using dynamic programming to precompute the maximum height to the left and right of each element.

**Intuition:**

1. Precompute the `left_max` array such that `left_max[i]` is the maximum height from the start to `i`.
2. Precompute the `right_max` array such that `right_max[i]` is the maximum height from `i` to the end.
3. Calculate the trapped water similarly by using these precomputed arrays.

**Algorithm:**

1. Create arrays `left_max` and `right_max` each of size `n`.
2. Fill `left_max` such that `left_max[i]` contains the maximum height seen so far from the left up to index `i`.
3. Similarly, fill `right_max`.
4. Iterate over each bar to compute the water trapped using the values from `left_max` and `right_max`.

```typescript
function trap(height: number[]): number {
    const n = height.length;
    if (n === 0) return 0;
    
    const left_max = new Array(n).fill(0);
    const right_max = new Array(n).fill(0);
    
    left_max[0] = height[0];
    for (let i = 1; i < n; i++) {
        left_max[i] = Math.max(left_max[i - 1], height[i]);
    }
    
    right_max[n - 1] = height[n - 1];
    for (let i = n - 2; i >= 0; i--) {
        right_max[i] = Math.max(right_max[i + 1], height[i]);
    }
    
    let water = 0;
    for (let i = 0; i < n; i++) {
        // Calculate trapped water using precomputed values and add it to the total
        water += Math.min(left_max[i], right_max[i]) - height[i];
    }
    
    return water;
}
```

**Time Complexity:** \(O(n)\)  
**Space Complexity:** \(O(n)\)

---

### Two Pointers

The two pointers technique further optimizes the approach in terms of space complexity.

**Intuition:**

1. Use two pointers, `left` and `right`, to represent the bounds of the height array.
2. Track two variables `left_max` and `right_max` to keep the maximum heights seen so far from the left and right respectively.
3. Move the pointers towards each other, deciding how much water is trapped based on the minimum of `left_max` and `right_max`.

**Algorithm:**

1. Initialize two pointers `left` at the beginning, and `right` at the end of the height array.
2. Initialize `left_max` and `right_max` to `0`.
3. While `left` is less than `right`:
   - If `height[left]` is less than `height[right]`, use `left`:
     - If `height[left]` is greater than or equal to `left_max`, update `left_max`.
     - Else, calculate water trapped and add it to the total.
     - Move `left` pointer to the right.
   - Otherwise, use `right`:
     - If `height[right]` is greater than or equal to `right_max`, update `right_max`.
     - Else, calculate water trapped and add it to the total.
     - Move `right` pointer to the left.

```typescript
function trap(height: number[]): number {
    let left = 0, right = height.length - 1;
    let left_max = 0, right_max = 0;
    let water = 0;

    while (left < right) {
        if (height[left] < height[right]) {
            if (height[left] >= left_max) {
                left_max = height[left]; // Update left_max
            } else {
                water += left_max - height[left]; // Calculate trapped water
            }
            left++;
        } else {
            if (height[right] >= right_max) {
                right_max = height[right]; // Update right_max
            } else {
                water += right_max - height[right]; // Calculate trapped water
            }
            right--;
        }
    }

    return water;
}
```

**Time Complexity:** \(O(n)\)  
**Space Complexity:** \(O(1)\)

