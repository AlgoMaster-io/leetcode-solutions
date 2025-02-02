# [Leetcode 283: Move Zeroes](https://leetcode.com/problems/move-zeroes/)

## Approaches:
1. [Brute Force Move and Shift](#brute-force-move-and-shift)
2. [Two-Pointer Approach](#two-pointer-approach)
3. [Optimized In-Place Zero Fill](#optimized-in-place-zero-fill)

---

### 1. Brute Force Move and Shift

#### Intuition:
The brute force approach involves iterating through the array and shifting non-zero elements towards the beginning while tracking the count of zeros. We will then fill the remainder of the array with zeros. This approach is straightforward but not optimal since we make multiple passes over the array.

#### Java Code:
```java
public void moveZeroes(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    int j = 0;

    // First pass: accumulate non-zero elements
    for (int i = 0; i < n; i++) {
        if (nums[i] != 0) {
            result[j] = nums[i];
            j++;
        }
    }

    // Second pass: fill remaining positions with zeroes
    for (; j < n; j++) {
        result[j] = 0;
    }

    // Copy back to original array
    System.arraycopy(result, 0, nums, 0, n);
}
```

#### Complexity Analysis:
- **Time Complexity:** O(n), where n is the length of the array.
- **Space Complexity:** O(n), due to the use of additional array.

---

### 2. Two-Pointer Approach

#### Intuition:
A more efficient approach uses the two-pointer technique. One pointer keeps track of the position to insert a non-zero element, while the other iterates through the array. When a non-zero element is found, it swaps positions with the element at the insertion pointer. This unfolds elegantly within a single pass, minimizing operations.

#### Java Code:
```java
public void moveZeroes(int[] nums) {
    int insertPos = 0; // Points to the index where the next non-zero should be placed

    // Iterate through the array
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            // Swap only if necessary, insertPos can be equal to i
            nums[insertPos] = nums[i];
            insertPos++;
        }
    }

    // Fill the rest of the array with zeroes
    while (insertPos < nums.length) {
        nums[insertPos] = 0;
        insertPos++;
    }
}
```

#### Complexity Analysis:
- **Time Complexity:** O(n), where n is the length of the array.
- **Space Complexity:** O(1), since we are modifying the array in place without using additional storage.

---

### 3. Optimized In-Place Zero Fill

#### Intuition:
We can fine-tune our previous two-pointer approach by swapping in place. We iterate over the array, and whenever a non-zero is found, we swap it with the element at the current zero position (`lastZeroPosition`). This ensures non-zero elements are pushed forward optimally.

#### Java Code:
```java
public void moveZeroes(int[] nums) {
    int index = 0;
    // First pass - move all non-zero numbers to front
    for(int num: nums) {
        if(num != 0) {
            nums[index++] = num;
        }
    }
    // Second pass - fill remaining with zeros
    while(index < nums.length) {
        nums[index++] = 0;
    }
}
```

#### Complexity Analysis:
- **Time Complexity:** O(n), as the array is traversed once.
- **Space Complexity:** O(1), no additional data structures are used.

Each of these methods incrementally improves upon the previous in terms of time complexity, space complexity, and operational efficiency, making the last approach often preferred in practice.

