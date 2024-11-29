# 307. [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approach 1: Brute Force

### Solution
csharp
```csharp
// Time Complexity for update: O(1)
// Time Complexity for sumRange: O(n)
// Space Complexity: O(n)
public class NumArray {
    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
    }

    public void Update(int index, int val) {
        // Directly update the value at the given index
        nums[index] = val;
    }

    public int SumRange(int i, int j) {
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
csharp
```csharp
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
public class NumArray {
    private int[] nums;
    private int[] segmentTree;

    public NumArray(int[] nums) {
        this.nums = nums;
        int n = nums.Length;
        segmentTree = new int[4 * n];
        BuildTree(0, 0, n - 1);
    }

    private void BuildTree(int treeIndex, int left, int right) {
        if (left == right) {
            // Leaf node stores the original array value
            segmentTree[treeIndex] = nums[left];
            return;
        }
        int mid = left + (right - left) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        BuildTree(leftChild, left, mid);
        BuildTree(rightChild, mid + 1, right);
        // Current node store the sum of both segments
        segmentTree[treeIndex] = segmentTree[leftChild] + segmentTree[rightChild];
    }

    public void Update(int index, int val) {
        UpdateRecursive(0, 0, nums.Length - 1, index, val);
    }

    private void UpdateRecursive(int treeIndex, int left, int right, int index, int val) {
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
            UpdateRecursive(leftChild, left, mid, index, val);
        } else {
            UpdateRecursive(rightChild, mid + 1, right, index, val);
        }
        // Recalculate the current node's sum after the update
        segmentTree[treeIndex] = segmentTree[leftChild] + segmentTree[rightChild];
    }

    public int SumRange(int i, int j) {
        return SumRangeRecursive(0, 0, nums.Length - 1, i, j);
    }

    private int SumRangeRecursive(int treeIndex, int left, int right, int i, int j) {
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
        return SumRangeRecursive(leftChild, left, mid, i, j) +
               SumRangeRecursive(rightChild, mid + 1, right, i, j);
    }
}
```

## Approach 3: Binary Indexed Tree (Fenwick Tree)

### Solution
csharp
```csharp
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
public class NumArray {
    private int[] nums;
    private int[] bit;
    private int n;

    public NumArray(int[] nums) {
        this.nums = nums;
        n = nums.Length;
        bit = new int[n + 1];
        for (int i = 0; i < n; i++) {
            Init(i, nums[i]);
        }
    }

    private void Init(int index, int val) {
        index++;
        while (index <= n) {
            // Update the binary indexed tree with the value
            bit[index] += val;
            index += index & -index;
        }
    }

    public void Update(int index, int val) {
        int delta = val - nums[index];
        nums[index] = val; // Update the original array
        Init(index, delta); // Update the BIT with the difference
    }

    private int GetSum(int index) {
        int sum = 0;
        index++;
        while (index > 0) {
            sum += bit[index]; // Accumulate the sum from the BIT
            index -= index & -index;
        }
        return sum;
    }

    public int SumRange(int i, int j) {
        return GetSum(j) - GetSum(i - 1);
    }
}
```

