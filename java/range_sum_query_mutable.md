# 307. [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approach 1: Brute Force

### Solution
```java
// Time Complexity for update: O(1)
// Time Complexity for sumRange: O(n)
// Space Complexity: O(n)
public class NumArray {
    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
    }

    public void update(int index, int val) {
        // Directly update the value at the given index
        nums[index] = val;
    }

    public int sumRange(int i, int j) {
        int sum = 0;
        // Calculate the sum from index i to j
        for (int k = i; k <= j; k++) {
            sum += nums[k];
        }
        return sum;
    }
}
```

## Approach 2: Segment Tree

### Solution
```java
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
public class NumArray {
    private int[] nums;
    private int[] segmentTree;

    public NumArray(int[] nums) {
        this.nums = nums;
        int n = nums.length;
        segmentTree = new int[4 * n];
        buildTree(0, 0, n - 1);
    }

    private void buildTree(int treeIndex, int left, int right) {
        if (left == right) {
            // Leaf node stores the original array value
            segmentTree[treeIndex] = nums[left];
            return;
        }
        int mid = left + (right - left) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        buildTree(leftChild, left, mid);
        buildTree(rightChild, mid + 1, right);
        // Current node store the sum of both segments
        segmentTree[treeIndex] = segmentTree[leftChild] + segmentTree[rightChild];
    }

    public void update(int index, int val) {
        updateRecursive(0, 0, nums.length - 1, index, val);
    }

    private void updateRecursive(int treeIndex, int left, int right, int index, int val) {
        if (left == right) {
            // Update the value in the segment tree and the original array
            segmentTree[treeIndex] = val;
            nums[index] = val;
            return;
        }
        int mid = left + (right - left) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        if (index <= mid) {
            updateRecursive(leftChild, left, mid, index, val);
        } else {
            updateRecursive(rightChild, mid + 1, right, index, val);
        }
        // Recalculate the current node's sum after the update
        segmentTree[treeIndex] = segmentTree[leftChild] + segmentTree[rightChild];
    }

    public int sumRange(int i, int j) {
        return sumRangeRecursive(0, 0, nums.length - 1, i, j);
    }

    private int sumRangeRecursive(int treeIndex, int left, int right, int i, int j) {
        if (left > j || right < i) {
            return 0; // Range completely outside
        }
        if (i <= left && right <= j) {
            return segmentTree[treeIndex]; // Range completely inside
        }
        int mid = left + (right - left) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        // Partial overlap
        return sumRangeRecursive(leftChild, left, mid, i, j) + 
               sumRangeRecursive(rightChild, mid + 1, right, i, j);
    }
}
```

## Approach 3: Binary Indexed Tree (Fenwick Tree)

### Solution
```java
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
public class NumArray {
    private int[] nums;
    private int[] bit;
    private int n;

    public NumArray(int[] nums) {
        this.nums = nums;
        n = nums.length;
        bit = new int[n + 1];
        for (int i = 0; i < n; i++) {
            init(i, nums[i]);
        }
    }

    private void init(int index, int val) {
        index++;
        while (index <= n) {
            // Update the binary indexed tree with the value
            bit[index] += val;
            index += index & -index;
        }
    }

    public void update(int index, int val) {
        int delta = val - nums[index];
        nums[index] = val; // Update the original array
        init(index, delta); // Update the BIT with the difference
    }

    private int getSum(int index) {
        int sum = 0;
        index++;
        while (index > 0) {
            sum += bit[index]; // Accumulate the sum from the BIT
            index -= index & -index;
        }
        return sum;
    }

    public int sumRange(int i, int j) {
        return getSum(j) - getSum(i - 1);
    }
}
```

