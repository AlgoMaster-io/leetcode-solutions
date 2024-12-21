# [307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Fenwick Tree (Binary Indexed Tree) Approach](#fenwick-tree-binary-indexed-tree-approach)
3. [Segment Tree Approach](#segment-tree-approach)

### Brute Force Approach

**Intuition:**

The brute force approach directly simulates the operations. For updating an element, we change its value in the array. For querying the sum, we iterate through the range and sum up elements. Though simple, this approach is inefficient for frequent queries or updates.

**Steps:**

1. **Update Operation:** Directly update the value at the given index.
2. **Query Sum:** Traverse the array from `left` to `right` and compute the sum.

**Time Complexity:** 
- `update`: \(O(1)\)
- `sumRange`: \(O(n)\)

**Space Complexity:** \(O(1)\)

```python
class NumArray:
    def __init__(self, nums):
        self.nums = nums

    def update(self, index, val):
        # Directly assign the value at the specified index
        self.nums[index] = val

    def sumRange(self, left, right):
        # Sum the elements over the specified range inclusively
        return sum(self.nums[left:right + 1])
```

### Fenwick Tree (Binary Indexed Tree) Approach

**Intuition:**

A Fenwick Tree allows both updates and prefix sums in logarithmic time. By maintaining an auxiliary array that efficiently updates elements and calculates cumulative sums, we can optimize the process significantly over the brute force method.

**Steps:**

1. Build a Fenwick Tree for the initial array of numbers.
2. For `update`, adjust the differences in the Fenwick Tree.
3. For `sumRange`, retrieve prefix sums using the Fenwick Tree and compute the range sum.

**Time Complexity:** 
- `update`: \(O(\log n)\)
- `sumRange`: \(O(\log n)\)

**Space Complexity:** \(O(n)\)

```python
class NumArray:
    def __init__(self, nums):
        # Initialize an array to store numbers
        self.nums = nums
        # Create a Fenwick Tree with an additional slot for ease of calculations
        self.fenwick_tree = [0] * (len(nums) + 1)
        
        for i, num in enumerate(nums):
            self._updateFenwickTree(i + 1, num)
    
    def _updateFenwickTree(self, index, delta):
        # Update the Fenwick Tree with the delta change
        while index < len(self.fenwick_tree):
            self.fenwick_tree[index] += delta
            index += index & -index
    
    def update(self, index, val):
        # Calculate delta between new value and current value
        delta = val - self.nums[index]
        # Update the number array
        self.nums[index] = val
        # Reflect this change in the Fenwick Tree
        self._updateFenwickTree(index + 1, delta)
    
    def _prefixSum(self, index):
        # Calculate prefix sum up to the given index
        result = 0
        while index > 0:
            result += self.fenwick_tree[index]
            index -= index & -index
        return result
    
    def sumRange(self, left, right):
        # Use prefix sums to calculate sum of the range
        return self._prefixSum(right + 1) - self._prefixSum(left)
```

### Segment Tree Approach

**Intuition:**

A Segment Tree offers not only logarithmic updates and queries, but also more flexibility in handling arbitrary ranges. Building a tree upfront creates nodes that store information for a span of elements, thus letting us merge information effortlessly during both updates and queries.

**Steps:**

1. Build a Segment Tree from the contents of the array.
2. On update, propagate changes upwards in the tree.
3. For `sumRange`, aggregate values selectively based on the current segment's reach.

**Time Complexity:** 
- `update`: \(O(\log n)\)
- `sumRange`: \(O(\log n)\)

**Space Complexity:** \(O(n)\)

```python
class NumArray:
    def __init__(self, nums):
        self.nums = nums
        self.segment_tree = [0] * (4 * len(nums))
        if nums:
            self.buildTree(0, 0, len(nums) - 1)
        
    def buildTree(self, node, start, end):
        # Recursively build the segment tree
        if start == end:
            # Leaf node
            self.segment_tree[node] = self.nums[start]
        else:
            mid = (start + end) // 2
            left_node = 2 * node + 1
            right_node = 2 * node + 2
            self.buildTree(left_node, start, mid)
            self.buildTree(right_node, mid + 1, end)
            self.segment_tree[node] = self.segment_tree[left_node] + self.segment_tree[right_node]
    
    def updateTree(self, node, start, end, idx, val):
        # Recursively update the segment tree
        if start == end:
            self.nums[idx] = val
            self.segment_tree[node] = val
        else:
            mid = (start + end) // 2
            left_node = 2 * node + 1
            right_node = 2 * node + 2
            if start <= idx <= mid:
                self.updateTree(left_node, start, mid, idx, val)
            else:
                self.updateTree(right_node, mid + 1, end, idx, val)
            self.segment_tree[node] = self.segment_tree[left_node] + self.segment_tree[right_node]
    
    def queryTree(self, node, start, end, L, R):
        # Recursively query the segment tree
        if R < start or end < L:
            return 0
        if L <= start and end <= R:
            return self.segment_tree[node]
        mid = (start + end) // 2
        left_sum = self.queryTree(2 * node + 1, start, mid, L, R)
        right_sum = self.queryTree(2 * node + 2, mid + 1, end, L, R)
        return left_sum + right_sum
    
    def update(self, index, val):
        self.updateTree(0, 0, len(self.nums) - 1, index, val)
    
    def sumRange(self, left, right):
        return self.queryTree(0, 0, len(self.nums) - 1, left, right)
```

In these solutions, we have explored a basic yet inefficient brute force method, an efficient and easy-to-implement Fenwick Tree approach, and a powerful Segment Tree strategy, striking a balance between performance and flexibility.

