# [Leetcode 307: Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approaches:
1. [Brute Force - Update and Query in O(n)](#1-brute-force---update-and-query-in-on)
2. [Segment Tree - Update and Query in O(log n)](#2-segment-tree---update-and-query-in-olog-n)
3. [Binary Indexed Tree (Fenwick Tree) - Update and Query in O(log n)](#3-binary-indexed-tree-fenwick-tree---update-and-query-in-olog-n)

---

### 1. Brute Force - Update and Query in O(n)

**Intuition:**

The brute force solution is straightforward. We maintain the array as-is and perform the `update` operation by directly updating the element at the given index. For the `sumRange` operation, we simply iterate over the range `[i, j]` to calculate the sum.

**Code:**

```typescript
class NumArray {
    private nums: number[];

    constructor(nums: number[]) {
        this.nums = nums;
    }

    update(index: number, val: number): void {
        // Directly update the value at the given index
        this.nums[index] = val;
    }

    sumRange(left: number, right: number): number {
        // Calculate the sum by iterating over the range [left, right]
        let sum = 0;
        for (let i = left; i <= right; i++) {
            sum += this.nums[i];
        }
        return sum;
    }
}
```

**Complexity:**

- Time Complexity:
  - `update`: O(1) for updating one element.
  - `sumRange`: O(n) for summing between two indices.
- Space Complexity: O(1) additional space apart from input.

---

### 2. Segment Tree - Update and Query in O(log n)

**Intuition:**

Segment Tree is a binary tree used for storing intervals or segments. It allows querying the sum of an interval efficiently while also allowing updates. The tree is built in such a way that leaves represent the elements of the array, and each internal node represents the sum of a segment.

**Code:**

```typescript
class NumArray {
    private nums: number[];
    private tree: number[];

    constructor(nums: number[]) {
        this.nums = nums;
        const n = nums.length;
        this.tree = Array(4 * n).fill(0);
        this.buildTree(0, 0, n - 1);
    }

    private buildTree(node: number, start: number, end: number): void {
        if (start === end) {
            // Leaf node
            this.tree[node] = this.nums[start];
        } else {
            const mid = Math.floor((start + end) / 2);
            const leftChild = 2 * node + 1;
            const rightChild = 2 * node + 2;
            this.buildTree(leftChild, start, mid);
            this.buildTree(rightChild, mid + 1, end);
            this.tree[node] = this.tree[leftChild] + this.tree[rightChild];
        }
    }

    update(index: number, val: number): void {
        this.updateTree(0, 0, this.nums.length - 1, index, val);
    }

    private updateTree(node: number, start: number, end: number, index: number, val: number): void {
        if (start === end) {
            // Leaf node
            this.nums[index] = val;
            this.tree[node] = val;
        } else {
            const mid = Math.floor((start + end) / 2);
            const leftChild = 2 * node + 1;
            const rightChild = 2 * node + 2;
            if (index <= mid) {
                this.updateTree(leftChild, start, mid, index, val);
            } else {
                this.updateTree(rightChild, mid + 1, end, index, val);
            }
            this.tree[node] = this.tree[leftChild] + this.tree[rightChild];
        }
    }

    sumRange(left: number, right: number): number {
        return this.queryTree(0, 0, this.nums.length - 1, left, right);
    }

    private queryTree(node: number, start: number, end: number, left: number, right: number): number {
        if (left > end || right < start) {
            return 0;
        }
        if (left <= start && end <= right) {
            return this.tree[node];
        }
        const mid = Math.floor((start + end) / 2);
        const leftSum = this.queryTree(2 * node + 1, start, mid, left, right);
        const rightSum = this.queryTree(2 * node + 2, mid + 1, end, left, right);
        return leftSum + rightSum;
    }
}
```

**Complexity:**

- Time Complexity:
  - `update`: O(log n) because of tree traversal.
  - `sumRange`: O(log n) because of tree traversal.
- Space Complexity: O(n) for the segment tree.

---

### 3. Binary Indexed Tree (Fenwick Tree) - Update and Query in O(log n)

**Intuition:**

A Binary Indexed Tree (BIT) or Fenwick Tree is a data structure that provides efficient methods for cumulative frequency tables. It supports point updates and prefix sum operations with logarithmic time complexity. For mutable range sum queries, it acts as a more space-efficient version than a segment tree.

**Code:**

```typescript
class NumArray {
    private nums: number[];
    private bit: number[];

    constructor(nums: number[]) {
        this.nums = nums;
        this.bit = new Array(nums.length + 1).fill(0);

        for (let i = 0; i < nums.length; i++) {
            this.updateBIT(i + 1, nums[i]);
        }
    }

    private updateBIT(index: number, val: number): void {
        while (index < this.bit.length) {
            this.bit[index] += val;
            index += index & -index;
        }
    }

    update(index: number, val: number): void {
        // Calculate the difference and update the Binary Indexed Tree
        const diff = val - this.nums[index];
        this.nums[index] = val;
        this.updateBIT(index + 1, diff);
    }

    private prefixSum(index: number): number {
        let sum = 0;
        while (index > 0) {
            sum += this.bit[index];
            index -= index & -index;
        }
        return sum;
    }

    sumRange(left: number, right: number): number {
        // Get the sum of the range by differences of prefix sums
        return this.prefixSum(right + 1) - this.prefixSum(left);
    }
}
```

**Complexity:**

- Time Complexity:
  - `update`: O(log n) for updating the BIT.
  - `sumRange`: O(log n) for calculating range sum with BIT.
- Space Complexity: O(n) for the BIT.

---

These are the detailed solutions and explanations for "Range Sum Query - Mutable" using three different approaches: Brute Force, Segment Tree, and Binary Indexed Tree. Each option has its own trade-offs in terms of time and space complexities, providing you choices based on constraints and requirements.

