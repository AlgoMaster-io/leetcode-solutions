# [Leetcode 307: Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approaches
1. [Brute Force](#brute-force)
2. [Segment Tree](#segment-tree)
3. [Binary Indexed Tree (Fenwick Tree)](#binary-indexed-tree-fenwick-tree)

---

## Brute Force

### Intuition
The simplest approach is to directly update the array for any update operation and calculate the range sum by iterating through the specified range. While straightforward, this method becomes inefficient, particularly for large arrays due to the linear time complexity for each query.

### Implementation

```csharp
public class NumArray {
    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
    }
    
    public void Update(int index, int val) {
        // Directly update the value at the specified index
        nums[index] = val;
    }
    
    public int SumRange(int left, int right) {
        // Calculate the sum by iterating through the range
        int sum = 0;
        for (int i = left; i <= right; i++) {
            sum += nums[i];
        }
        return sum;
    }
}
```

### Time Complexity
- `Update`: O(1) for updating one element.
- `SumRange`: O(n) where n is the size of the range `[left, right]`.

### Space Complexity
- O(1), apart from the input array.

---

## Segment Tree

### Intuition
By using a segment tree, we can efficiently perform both update and range sum queries. Segment trees allow us to break the array into segments, store results for these segments, and recombine them quickly.

### Implementation

```csharp
public class NumArray {
    private int[] segmentTree;
    private int n;

    public NumArray(int[] nums) {
        if (nums.Length > 0) {
            n = nums.Length;
            segmentTree = new int[4 * n];
            BuildSegmentTree(nums, 0, 0, n - 1);
        }
    }
    
    private void BuildSegmentTree(int[] nums, int treeIndex, int low, int high) {
        if (low == high) {
            segmentTree[treeIndex] = nums[low];
            return;
        }
        
        int mid = low + (high - low) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        BuildSegmentTree(nums, leftChild, low, mid);
        BuildSegmentTree(nums, rightChild, mid + 1, high);
        segmentTree[treeIndex] = segmentTree[leftChild] + segmentTree[rightChild];
    }
    
    public void Update(int index, int val) {
        UpdateTree(0, 0, n - 1, index, val);
    }
    
    private void UpdateTree(int treeIndex, int low, int high, int index, int val) {
        if (low == high) {
            segmentTree[treeIndex] = val;
            return;
        }
        
        int mid = low + (high - low) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        
        if (index <= mid) {
            UpdateTree(leftChild, low, mid, index, val);
        } else {
            UpdateTree(rightChild, mid + 1, high, index, val);
        }
        
        segmentTree[treeIndex] = segmentTree[leftChild] + segmentTree[rightChild];
    }
    
    public int SumRange(int left, int right) {
        return QueryTree(0, 0, n - 1, left, right);
    }
    
    private int QueryTree(int treeIndex, int low, int high, int rangeLow, int rangeHigh) {
        if (low > rangeHigh || high < rangeLow) {
            return 0;
        }
        
        if (low >= rangeLow && high <= rangeHigh) {
            return segmentTree[treeIndex];
        }
        
        int mid = low + (high - low) / 2;
        int leftSum = QueryTree(2 * treeIndex + 1, low, mid, rangeLow, rangeHigh);
        int rightSum = QueryTree(2 * treeIndex + 2, mid + 1, high, rangeLow, rangeHigh);
        
        return leftSum + rightSum;
    }
}
```

### Time Complexity
- `Update`: O(log n) due to the tree height.
- `SumRange`: O(log n) due to the tree traversal.

### Space Complexity
- O(n) for the segment tree storage.

---

## Binary Indexed Tree (Fenwick Tree)

### Intuition
A Binary Indexed Tree (or Fenwick Tree) provides another efficient method to handle updates and prefix sums. It offers a more compact representation compared to a segment tree with a simple array structure.

### Implementation

```csharp
public class NumArray {
    private int[] fenwickTree;
    private int[] nums;
    private int n;

    public NumArray(int[] nums) {
        this.nums = nums;
        n = nums.Length;
        fenwickTree = new int[n + 1];
        for (int i = 0; i < n; i++) {
            UpdateFenwickTree(i + 1, nums[i]);
        }
    }
    
    public void Update(int index, int val) {
        int delta = val - nums[index];
        nums[index] = val;
        UpdateFenwickTree(index + 1, delta);
    }
    
    private void UpdateFenwickTree(int index, int delta) {
        while (index <= n) {
            fenwickTree[index] += delta;
            index += index & -index;
        }
    }
    
    public int SumRange(int left, int right) {
        return PrefixSum(right + 1) - PrefixSum(left);
    }
    
    private int PrefixSum(int index) {
        int sum = 0;
        while (index > 0) {
            sum += fenwickTree[index];
            index -= index & -index;
        }
        return sum;
    }
}
```

### Time Complexity
- `Update`: O(log n) due to the operations on the BIT.
- `SumRange`: O(log n) due to the prefix sum calculation.

### Space Complexity
- O(n) for the binary indexed tree storage.

---

Each approach offers a trade-off between simplicity and efficiency. The choice depends on the frequency of updates versus queries and the constraints of the problem. The Binary Indexed Tree offers a compact yet efficient solution for this range sum problem.

