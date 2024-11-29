# 307. [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approach 1: Brute Force

### Solution
```javascript
// Time Complexity for update: O(1)
// Time Complexity for sumRange: O(n)
// Space Complexity: O(n)
class NumArray {
    constructor(nums) {
        this.nums = nums;
    }

    update(index, val) {
        // Directly update the value at the given index
        this.nums[index] = val;
    }

    sumRange(i, j) {
        let sum = 0;
        // Calculate the sum from index i to j
        for (let k = i; k <= j; k++) {
            sum += this.nums[k];
        }
        return sum;
    }
}
```

## Approach 2: Segment Tree

### Solution
```javascript
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
class NumArray {
    constructor(nums) {
        this.nums = nums;
        this.n = nums.length;
        this.segmentTree = new Array(4 * this.n).fill(0);
        this.buildTree(0, 0, this.n - 1);
    }

    buildTree(treeIndex, left, right) {
        if (left === right) {
            // Leaf node stores the original array value
            this.segmentTree[treeIndex] = this.nums[left];
            return;
        }
        const mid = left + Math.floor((right - left) / 2);
        const leftChild = 2 * treeIndex + 1;
        const rightChild = 2 * treeIndex + 2;
        this.buildTree(leftChild, left, mid);
        this.buildTree(rightChild, mid + 1, right);
        // Current node store the sum of both segments
        this.segmentTree[treeIndex] = this.segmentTree[leftChild] + this.segmentTree[rightChild];
    }

    update(index, val) {
        this.updateRecursive(0, 0, this.n - 1, index, val);
    }

    updateRecursive(treeIndex, left, right, index, val) {
        if (left === right) {
            // Update the value in the segment tree and the original array
            this.segmentTree[treeIndex] = val;
            this.nums[index] = val;
            return;
        }
        const mid = left + Math.floor((right - left) / 2);
        const leftChild = 2 * treeIndex + 1;
        const rightChild = 2 * treeIndex + 2;
        if (index <= mid) {
            this.updateRecursive(leftChild, left, mid, index, val);
        } else {
            this.updateRecursive(rightChild, mid + 1, right, index, val);
        }
        // Recalculate the current node's sum after the update
        this.segmentTree[treeIndex] = this.segmentTree[leftChild] + this.segmentTree[rightChild];
    }

    sumRange(i, j) {
        return this.sumRangeRecursive(0, 0, this.n - 1, i, j);
    }

    sumRangeRecursive(treeIndex, left, right, i, j) {
        if (left > j || right < i) {
            return 0; // Range completely outside
        }
        if (i <= left && right <= j) {
            return this.segmentTree[treeIndex]; // Range completely inside
        }
        const mid = left + Math.floor((right - left) / 2);
        const leftChild = 2 * treeIndex + 1;
        const rightChild = 2 * treeIndex + 2;
        // Partial overlap
        return this.sumRangeRecursive(leftChild, left, mid, i, j) + 
               this.sumRangeRecursive(rightChild, mid + 1, right, i, j);
    }
}
```

## Approach 3: Binary Indexed Tree (Fenwick Tree)

### Solution
```javascript
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
class NumArray {
    constructor(nums) {
        this.nums = nums;
        this.n = nums.length;
        this.bit = new Array(this.n + 1).fill(0);
        for (let i = 0; i < this.n; i++) {
            this.init(i, nums[i]);
        }
    }

    init(index, val) {
        index++;
        while (index <= this.n) {
            // Update the binary indexed tree with the value
            this.bit[index] += val;
            index += index & -index;
        }
    }

    update(index, val) {
        const delta = val - this.nums[index];
        this.nums[index] = val; // Update the original array
        this.init(index, delta); // Update the BIT with the difference
    }

    getSum(index) {
        let sum = 0;
        index++;
        while (index > 0) {
            sum += this.bit[index]; // Accumulate the sum from the BIT
            index -= index & -index;
        }
        return sum;
    }

    sumRange(i, j) {
        return this.getSum(j) - this.getSum(i - 1);
    }
}
```

