# 307. [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approach 1: Brute Force

### Solution
typescript
```typescript
// Time Complexity for update: O(1)
// Time Complexity for sumRange: O(n)
// Space Complexity: O(n)
class NumArray {
    private nums: number[];

    constructor(nums: number[]) {
        this.nums = nums;
    }

    update(index: number, val: number): void {
        // Directly update the value at the given index
        this.nums[index] = val;
    }

    sumRange(i: number, j: number): number {
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
typescript
```typescript
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
class NumArray {
    private nums: number[];
    private segmentTree: number[];

    constructor(nums: number[]) {
        this.nums = nums;
        const n = nums.length;
        this.segmentTree = new Array(4 * n).fill(0);
        this.buildTree(0, 0, n - 1);
    }

    private buildTree(treeIndex: number, left: number, right: number): void {
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

    update(index: number, val: number): void {
        this.updateRecursive(0, 0, this.nums.length - 1, index, val);
    }

    private updateRecursive(treeIndex: number, left: number, right: number, index: number, val: number): void {
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

    sumRange(i: number, j: number): number {
        return this.sumRangeRecursive(0, 0, this.nums.length - 1, i, j);
    }

    private sumRangeRecursive(treeIndex: number, left: number, right: number, i: number, j: number): number {
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
typescript
```typescript
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
class NumArray {
    private nums: number[];
    private bit: number[];
    private n: number;

    constructor(nums: number[]) {
        this.nums = nums;
        this.n = nums.length;
        this.bit = new Array(this.n + 1).fill(0);
        for (let i = 0; i < this.n; i++) {
            this.init(i, nums[i]);
        }
    }

    private init(index: number, val: number): void {
        index++;
        while (index <= this.n) {
            // Update the binary indexed tree with the value
            this.bit[index] += val;
            index += index & -index;
        }
    }

    update(index: number, val: number): void {
        const delta = val - this.nums[index];
        this.nums[index] = val; // Update the original array
        this.init(index, delta); // Update the BIT with the difference
    }

    private getSum(index: number): number {
        let sum = 0;
        index++;
        while (index > 0) {
            sum += this.bit[index]; // Accumulate the sum from the BIT
            index -= index & -index;
        }
        return sum;
    }

    sumRange(i: number, j: number): number {
        return this.getSum(j) - this.getSum(i - 1);
    }
}
```

