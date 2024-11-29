# 307. [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approach 1: Brute Force

### Solution
python
```python
# Time Complexity for update: O(1)
# Time Complexity for sumRange: O(n)
# Space Complexity: O(n)

class NumArray:
    def __init__(self, nums):
        self.nums = nums

    def update(self, index, val):
        # Directly update the value at the given index
        self.nums[index] = val

    def sumRange(self, i, j):
        # Calculate the sum from index i to j
        return sum(self.nums[i:j+1])
```

## Approach 2: Segment Tree

### Solution
python
```python
# Time Complexity for update: O(log n)
# Time Complexity for sumRange: O(log n)
# Space Complexity: O(n)

class NumArray:
    def __init__(self, nums):
        self.nums = nums
        n = len(nums)
        self.segment_tree = [0] * (4 * n)
        self.build_tree(0, 0, n - 1)

    def build_tree(self, tree_index, left, right):
        if left == right:
            # Leaf node stores the original array value
            self.segment_tree[tree_index] = self.nums[left]
            return
        mid = left + (right - left) // 2
        left_child = 2 * tree_index + 1
        right_child = 2 * tree_index + 2
        self.build_tree(left_child, left, mid)
        self.build_tree(right_child, mid + 1, right)
        # Current node store the sum of both segments
        self.segment_tree[tree_index] = self.segment_tree[left_child] + self.segment_tree[right_child]

    def update(self, index, val):
        self.update_recursive(0, 0, len(self.nums) - 1, index, val)

    def update_recursive(self, tree_index, left, right, index, val):
        if left == right:
            # Update the value in the segment tree and the original array
            self.segment_tree[tree_index] = val
            self.nums[index] = val
            return
        mid = left + (right - left) // 2
        left_child = 2 * tree_index + 1
        right_child = 2 * tree_index + 2
        if index <= mid:
            self.update_recursive(left_child, left, mid, index, val)
        else:
            self.update_recursive(right_child, mid + 1, right, index, val)
        # Recalculate the current node's sum after the update
        self.segment_tree[tree_index] = self.segment_tree[left_child] + self.segment_tree[right_child]

    def sumRange(self, i, j):
        return self.sum_range_recursive(0, 0, len(self.nums) - 1, i, j)

    def sum_range_recursive(self, tree_index, left, right, i, j):
        if left > j or right < i:
            return 0 # Range completely outside
        if i <= left and right <= j:
            return self.segment_tree[tree_index] # Range completely inside
        mid = left + (right - left) // 2
        left_child = 2 * tree_index + 1
        right_child = 2 * tree_index + 2
        # Partial overlap
        return self.sum_range_recursive(left_child, left, mid, i, j) + \
               self.sum_range_recursive(right_child, mid + 1, right, i, j)
```

## Approach 3: Binary Indexed Tree (Fenwick Tree)

### Solution
python
```python
# Time Complexity for update: O(log n)
# Time Complexity for sumRange: O(log n)
# Space Complexity: O(n)

class NumArray:
    def __init__(self, nums):
        self.nums = nums
        self.n = len(nums)
        self.bit = [0] * (self.n + 1)
        for i in range(self.n):
            self.init(i, nums[i])

    def init(self, index, val):
        index += 1
        while index <= self.n:
            # Update the binary indexed tree with the value
            self.bit[index] += val
            index += index & -index

    def update(self, index, val):
        delta = val - self.nums[index]
        self.nums[index] = val # Update the original array
        self.init(index, delta) # Update the BIT with the difference

    def get_sum(self, index):
        sum_ = 0
        index += 1
        while index > 0:
            # Accumulate the sum from the BIT
            sum_ += self.bit[index]
            index -= index & -index
        return sum_

    def sumRange(self, i, j):
        return self.get_sum(j) - self.get_sum(i - 1)
```

