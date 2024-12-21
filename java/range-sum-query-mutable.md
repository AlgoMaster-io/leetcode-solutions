# [Leetcode 307: Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Segment Tree Approach](#segment-tree-approach)
- [Binary Indexed Tree (Fenwick Tree) Approach](#binary-indexed-tree-fenwick-tree-approach)

### Brute Force Approach

#### Intuition
- Each update or sumRange query operates in constant time when using a brute force method.
- On `update` operation, we simply change the value at the designated index.
- On `sumRange(i, j)` operation, we iterate through the array from index `i` to `j` and calculate the sum.
 
This approach sacrifices performance for simplicity, offering an easy-to-understand solution at the cost of increased time complexity for range queries.

#### Code

```java
class NumArray {
    private int[] nums;
    
    public NumArray(int[] nums) {
        this.nums = nums; // Store the input array
    }
    
    public void update(int index, int val) {
        nums[index] = val; // Directly update the value at index
    }
    
    public int sumRange(int left, int right) {
        int sum = 0;
        // Accumulate the sum from left to right index
        for (int i = left; i <= right; i++) {
            sum += nums[i];
        }
        return sum;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** 
  - `update`: O(1)
  - `sumRange`: O(n), where n is the number of elements between `left` and `right`.
- **Space Complexity:** O(1) beyond the input storage, as we do not use any additional data structures besides a simple array.

### Segment Tree Approach

#### Intuition
- A Segment Tree is designed specifically to solve range query problems efficiently.
- It pre-computes the range sums and supports efficient update and range sum operations.
- With Segment Trees, both `update` and `sumRange` operations can be conducted in O(log n) time, balancing between brute force direct access and high efficiency.

#### Code

```java
class NumArray {
    private int[] segmentTree;
    private int n;

    public NumArray(int[] nums) {
        if (nums.length == 0) return;
        this.n = nums.length;
        segmentTree = new int[n * 4];
        buildSegmentTree(nums, 0, 0, n - 1);
    }

    private void buildSegmentTree(int[] nums, int treeIndex, int lo, int hi) {
        if (lo == hi) {
            segmentTree[treeIndex] = nums[lo];
            return;
        }
        
        int mid = lo + (hi - lo) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        buildSegmentTree(nums, leftChild, lo, mid);
        buildSegmentTree(nums, rightChild, mid + 1, hi);
        segmentTree[treeIndex] = segmentTree[leftChild] + segmentTree[rightChild];
    }

    public void update(int index, int val) {
        updateSegmentTree(0, 0, n - 1, index, val);
    }

    private void updateSegmentTree(int treeIndex, int lo, int hi, int arrIndex, int val) {
        if (lo == hi) {
            segmentTree[treeIndex] = val;
            return;
        }
        
        int mid = lo + (hi - lo) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        
        if (arrIndex <= mid) {
            updateSegmentTree(leftChild, lo, mid, arrIndex, val);
        } else {
            updateSegmentTree(rightChild, mid + 1, hi, arrIndex, val);
        }
        segmentTree[treeIndex] = segmentTree[leftChild] + segmentTree[rightChild];
    }

    public int sumRange(int left, int right) {
        return querySegmentTree(0, 0, n - 1, left, right);
    }

    private int querySegmentTree(int treeIndex, int lo, int hi, int left, int right) {
        if (lo > right || hi < left) return 0;
        if (lo >= left && hi <= right) return segmentTree[treeIndex];
        
        int mid = lo + (hi - lo) / 2;
        int leftChild = 2 * treeIndex + 1;
        int rightChild = 2 * treeIndex + 2;
        if (right <= mid) return querySegmentTree(leftChild, lo, mid, left, right);
        else if (left > mid) return querySegmentTree(rightChild, mid + 1, hi, left, right);
        
        int leftSum = querySegmentTree(leftChild, lo, mid, left, right);
        int rightSum = querySegmentTree(rightChild, mid + 1, hi, left, right);
        return leftSum + rightSum;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** 
  - `update`: O(log n)
  - `sumRange`: O(log n)
- **Space Complexity:** O(n), used by the segment tree structure.

### Binary Indexed Tree (Fenwick Tree) Approach

#### Intuition
- A Binary Indexed Tree (or Fenwick Tree) is another advanced data structure that supports prefix sum operations efficiently, similar to Segment Trees.
- Fenwick Trees provide a simpler, more compact structure that allows both updates and prefix sum retrievals in O(log n) time.
  
#### Code 

```java
class NumArray {
    private int[] nums;
    private int[] BIT;
    private int n;
    
    public NumArray(int[] nums) {
        this.n = nums.length;
        this.nums = new int[n];
        System.arraycopy(nums, 0, this.nums, 0, n); // Copy the nums array
        BIT = new int[n + 1];
        for (int i = 0; i < n; i++) {
            init(i, nums[i]);
        }
    }
    
    private void init(int index, int val) {
        index++;
        while (index <= n) {
            BIT[index] += val;
            index += index & -index;
        }
    }
    
    public void update(int index, int val) {
        int diff = val - nums[index];
        nums[index] = val;
        init(index, diff);
    }
    
    public int sumRange(int left, int right) {
        return getSum(right) - getSum(left - 1);
    }
    
    private int getSum(int index) {
        int sum = 0;
        index++;
        while (index > 0) {
            sum += BIT[index];
            index -= index & -index;
        }
        return sum;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** 
  - `update`: O(log n)
  - `sumRange`: O(log n)
- **Space Complexity:** O(n), used by the Binary Indexed Tree structure.

Each of the approaches offers a different trade-off between simplicity and performance, with Segment Trees and Binary Indexed Trees providing efficient solutions for range sum queries with update operations.

