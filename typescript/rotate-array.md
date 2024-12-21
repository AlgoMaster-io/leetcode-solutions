# [Leetcode 189: Rotate Array](https://leetcode.com/problems/rotate-array/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Using Extra Array](#using-extra-array)
3. [Cyclic Replacements](#cyclic-replacements)
4. [Reverse Method (Optimal Approach)](#reverse-method)

---

### Brute Force Approach

**Intuition:**  
The most straightforward method involves rotating the array one step at a time, k times. For each step, we save the last element and shift all other elements to the right.

```typescript
function rotateBruteForce(nums: number[], k: number): void {
    const n = nums.length;
    // Reduce k as rotations exceed n will lead to repetition.
    k = k % n;
    
    for (let i = 0; i < k; i++) {
        // Save last element
        const last = nums[n - 1];
        
        // Move all elements to the right
        for (let j = n - 1; j > 0; j--) {
            nums[j] = nums[j - 1];
        }
        
        // Place last element at the start
        nums[0] = last;
    }
}
```

**Time Complexity:** O(n * k), where n is the number of elements and k is the number of rotations.  
**Space Complexity:** O(1).

---

### Using Extra Array

**Intuition:**  
An additional array can be used to rearrange elements as per their new positions. This method involves directly placing each element in its rotated position.

```typescript
function rotateWithExtraArray(nums: number[], k: number): void {
    const n = nums.length;
    k = k % n;  // Simplify k
    
    // Initialize a new array
    const rotated = new Array(n);
    
    // Place each element in its new position
    for (let i = 0; i < n; i++) {
        rotated[(i + k) % n] = nums[i];
    }
    
    // Copy back to the original array
    for (let i = 0; i < n; i++) {
        nums[i] = rotated[i];
    }
}
```

**Time Complexity:** O(n).  
**Space Complexity:** O(n) due to the auxiliary array.

---

### Cyclic Replacements

**Intuition:**  
This method involves putting each element in its final place, cycling through them. We shift elements one by one using an intermediary swap technique. This approach avoids unnecessary repositioning.

```typescript
function rotateCyclicReplacements(nums: number[], k: number): void {
    const n = nums.length;
    k = k % n;  // Optimize k

    let count = 0;  // To track rotation steps
    for (let start = 0; count < n; start++) {
        let current = start;
        let prevValue = nums[start];
        
        do {
            const nextIdx = (current + k) % n;
            // Swap elements to their new position in a circular manner
            const temp = nums[nextIdx];
            nums[nextIdx] = prevValue;
            prevValue = temp;
            current = nextIdx;
            count++;
        } while (start !== current);
    }
}
```

**Time Complexity:** O(n).  
**Space Complexity:** O(1) since no extra space is used other than variables.

---

### Reverse Method

**Intuition:**  
The most optimal method involves reversing sections of the array. First, reverse the entire array, then reverse the first `k` elements, and finally, reverse the last `n-k` elements. This operation naturally shifts elements into their rotated positions.

```typescript
function reverse(nums: number[], start: number, end: number): void {
    while (start < end) {
        // Swap elements at start and end
        const temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}

function rotateReverseMethod(nums: number[], k: number): void {
    const n = nums.length;
    k = k % n;  // Simplify k
    
    // Reverse the whole array
    reverse(nums, 0, n - 1);
    // Reverse first k elements
    reverse(nums, 0, k - 1);
    // Reverse the remaining n-k elements
    reverse(nums, k, n - 1);
}
```

**Time Complexity:** O(n).  
**Space Complexity:** O(1) as this approach makes in-place modifications without extra space.

---

**Note:** Choose the optimal solution based on constraints and environment, typically the reverse method for its balanced efficiency in both time and space.

