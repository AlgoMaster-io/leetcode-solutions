# [Leetcode 315: Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Binary Indexed Tree (BIT) Approach](#binary-indexed-tree-bit-approach)
3. [Merge Sort with Inversion Count Approach](#merge-sort-with-inversion-count-approach)

---

## Brute Force Approach

### Intuition

The most straightforward way to solve this problem is to iterate through each element in the array, and for each element, count the number of elements to its right that are smaller. This approach is simple to understand but is not efficient for larger datasets as it requires nested loops.

### Implementation

```javascript
function countSmaller(nums) {
    let counts = Array(nums.length).fill(0);

    for (let i = 0; i < nums.length; i++) {
        let count = 0;
        for (let j = i + 1; j < nums.length; j++) {
            // Count how many numbers are smaller than nums[i] after index i
            if (nums[j] < nums[i]) {
                count++;
            }
        }
        counts[i] = count; // Store the result for nums[i]
    }

    return counts;
}
```

### Time and Space Complexity
- **Time Complexity:** O(n^2), where n is the number of elements in the array since for each element we scan through the array.
- **Space Complexity:** O(n), for storing the result counts array.

---

## Binary Indexed Tree (BIT) Approach

### Intuition

To optimize the solution, we can use a Binary Indexed Tree, which helps efficiently update counts and calculate prefix sums. This data structure is well-suited for dynamic frequency counting and range queries, which is beneficial in counting smaller numbers.

### Implementation

```javascript
class BIT {
    constructor(size) {
        this.size = size;
        this.tree = Array(size + 1).fill(0);
    }
    
    update(index, value) {
        while (index <= this.size) {
            this.tree[index] += value;
            index += index & -index; // Move index to parent node
        }
    }
    
    query(index) {
        let sum = 0;
        while (index > 0) {
            sum += this.tree[index];
            index -= index & -index; // Move index to parent node
        }
        return sum;
    }
}

function countSmaller(nums) {
    let offset = 10000; // Offset negative values
    let size = 20001; // Range of [-10000, 10000]
    
    let counts = Array(nums.length).fill(0);
    let bit = new BIT(size);

    for (let i = nums.length - 1; i >= 0; i--) {
        // Query to get count of numbers less than the current number
        counts[i] = bit.query(nums[i] + offset);
        // Update BIT for the current index
        bit.update(nums[i] + offset + 1, 1);
    }

    return counts;
}
```

### Time and Space Complexity
- **Time Complexity:** O(n log n), where n is the number of elements in the array.
- **Space Complexity:** O(n), for the BIT data structure.

---

## Merge Sort with Inversion Count Approach

### Intuition

Another optimal approach is to use a modified merge sort technique, which naturally counts inversions (where nums[i] > nums[j] and i < j). During the merge step, we can count how many smaller elements exist after each element.

### Implementation

```javascript
function countSmaller(nums) {
    let counts = Array(nums.length).fill(0);
    let indices = nums.map((_, i) => i);

    mergeSort(nums, indices, 0, nums.length - 1, counts);

    return counts;
}

function mergeSort(nums, indices, left, right, counts) {
    if (right <= left) return;
    
    let mid = left + Math.floor((right - left) / 2);

    mergeSort(nums, indices, left, mid, counts);
    mergeSort(nums, indices, mid + 1, right, counts);

    merge(nums, indices, left, mid, right, counts);
}

function merge(nums, indices, left, mid, right, counts) {
    let temp = [];
    let i = left, j = mid + 1, rightCounter = 0;

    while (i <= mid && j <= right) {
        if (nums[indices[j]] < nums[indices[i]]) {
            rightCounter++;
            temp.push(indices[j++]);
        } else {
            counts[indices[i]] += rightCounter;
            temp.push(indices[i++]);
        }
    }

    while (i <= mid) {
        counts[indices[i]] += rightCounter;
        temp.push(indices[i++]);
    }

    while (j <= right) {
        temp.push(indices[j++]);
    }

    for (let k = left; k <= right; k++) {
        indices[k] = temp[k - left];
    }
}
```

### Time and Space Complexity
- **Time Complexity:** O(n log n), where n is the number of elements in the array.
- **Space Complexity:** O(n), due to the additional arrays used for indexing and sorting.

