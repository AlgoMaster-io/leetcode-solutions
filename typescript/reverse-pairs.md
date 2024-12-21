# [Leetcode 493: Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Fenwick Tree (Binary Indexed Tree)](#approach-2-fenwick-tree)
- [Approach 3: Merge Sort](#approach-3-merge-sort)

### Approach 1: Brute Force

#### Intuition
The brute force approach involves checking all pairs `(i, j)` where `i < j` and counting how many pairs satisfy the condition `nums[i] > 2 * nums[j]`. Although this solution checks every possibility, it will not perform well for large inputs due to high time complexity.

#### Implementation

```typescript
function reversePairs(nums: number[]): number {
    let count = 0;
    const n = nums.length;
    // Check all pairs (i, j) with i < j
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            // Check the condition
            if (nums[i] > 2 * nums[j]) {
                count++;
            }
        }
    }
    return count;
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(n^2)\) due to the nested loops which go through all pair combinations.
- **Space Complexity:** \(O(1)\) as we are using only a fixed amount of space.

### Approach 2: Fenwick Tree (Binary Indexed Tree)

#### Intuition
To reduce the complexity, we can use a Fenwick Tree to keep track of the elements seen so far and efficiently count elements that satisfy the reverse pair condition. We compress the array to make it fit in Fenwick Tree by converting it into a rank space.

#### Implementation

```typescript
class FenwickTree {
    tree: number[];
    
    constructor(size: number) {
        this.tree = Array(size + 1).fill(0);
    }

    update(index: number, delta: number) {
        while (index < this.tree.length) {
            this.tree[index] += delta;
            index += index & -index;
        }
    }

    query(index: number): number {
        let sum = 0;
        while (index > 0) {
            sum += this.tree[index];
            index -= index & -index;
        }
        return sum;
    }
}

function reversePairs(nums: number[]): number {
    let count = 0;
    
    // Convert nums to avoid negative indices in Fenwick Tree
    const sorted = Array.from(new Set(nums)).sort((a, b) => a - b);
    const getIndex = (val: number) => sorted.indexOf(val) + 1;

    const bit = new FenwickTree(sorted.length);
    
    // Traverse the nums array from right to left
    for (let num of nums.slice().reverse()) {
        // Count elements seen so far that are less than or equal to num/2
        count += bit.query(getIndex(num / 2));
        
        // Update Fenwick tree with current number rank
        bit.update(getIndex(num), 1);
    }

    return count;
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(n \log n)\) due to sorting and Fenwick Tree operations (`update` and `query`).
- **Space Complexity:** \(O(n)\) for storing the Fenwick Tree and the sorted ranks.

### Approach 3: Merge Sort

#### Intuition
Another optimized approach is a modified merge sort. During the merge step, we count reverse pairs with `nums[i] > 2 * nums[j]`. This approach takes advantage of the sorted nature of subarrays to efficiently count valid pairs.

#### Implementation

```typescript
function mergeAndCount(nums: number[], left: number, right: number): number {
    if (left >= right) return 0;

    const mid = Math.floor((left + right) / 2);
    let count = mergeAndCount(nums, left, mid) + mergeAndCount(nums, mid + 1, right);

    // Count reverse pairs in the sorted arrays
    let j = mid + 1;
    for (let i = left; i <= mid; i++) {
        while (j <= right && nums[i] > 2 * nums[j]) j++;
        count += j - (mid + 1);
    }

    // Merge the two sorted halves
    const sorted = [];
    let i = left, k = mid + 1;
    while (i <= mid && k <= right) {
        if (nums[i] <= nums[k]) {
            sorted.push(nums[i++]);
        } else {
            sorted.push(nums[k++]);
        }
    }
    while (i <= mid) sorted.push(nums[i++]);
    while (k <= right) sorted.push(nums[k++]);

    // Place sorted values back into original array
    for (let x = left; x <= right; x++) {
        nums[x] = sorted[x - left];
    }

    return count;
}

function reversePairs(nums: number[]): number {
    return mergeAndCount(nums, 0, nums.length - 1);
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(n \log n)\) due to the merge sort process.
- **Space Complexity:** \(O(n)\) due to auxiliary arrays used during the merge step. 

These solutions progressively improve the efficiency of finding the reverse pairs, from the straightforward brute-force method to more advanced algorithms utilizing data structures and divide-and-conquer techniques.

