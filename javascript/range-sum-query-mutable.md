# [Leetcode 307: Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Fenwick Tree (Binary Indexed Tree) Approach](#fenwick-tree-binary-indexed-tree-approach)
3. [Segment Tree Approach](#segment-tree-approach)

### Brute Force Approach

In this approach, we start with a simple brute force solution where each update operation simply updates the array, and each range sum query iterates from start to end indices to compute the sum. While this approach is straightforward, it is inefficient for large arrays and frequent queries, as it does not make use of any data structures to optimize repeated operations.

```javascript
class NumArray {
    constructor(nums) {
        this.nums = nums;
    }
    
    update(index, val) {
        // Directly update the element at the given index
        this.nums[index] = val;
    }
    
    sumRange(left, right) {
        let sum = 0;
        // Accumulate the sum from left to right index, inclusive
        for (let i = left; i <= right; i++) {
            sum += this.nums[i];
        }
        return sum;
    }
}
```
**Time Complexity:**
- `update`: O(1) time to update a single element.
- `sumRange`: O(n) time to calculate sum over a range `n`.
  
**Space Complexity:** O(n) where n is the number of elements in the array.

### Fenwick Tree (Binary Indexed Tree) Approach

The Fenwick Tree, also known as a Binary Indexed Tree (BIT), is a data structure that offers efficient methods for update and prefix sum queries. It leverages a tree structure to compress information about prefixes of a dataset.

#### Intuition
- We maintain an auxiliary structure (tree) that helps in efficiently performing prefix sum operations.
- When an update is performed, we update this structure efficiently to reflect changes.
- When a sum range is queried, we can leverage the structure to quickly compute the required sums.

```javascript
class NumArray {
    constructor(nums) {
        this.nums = nums;
        this.bit = Array(nums.length + 1).fill(0);
      
        // Initialize Binary Indexed Tree
        nums.forEach((num, idx) => this._add(idx + 1, num));
    }
    
    _add(index, val) {
        // Propagate the update through the Binary Indexed Tree
        while (index < this.bit.length) {
            this.bit[index] += val;
            index += index & -index;
        }
    }
    
    _prefixSum(index) {
        let sum = 0;
        // Compute prefix sum using BIT structure
        while (index > 0) {
            sum += this.bit[index];
            index -= index & -index;
        }
        return sum;
    }
    
    update(index, val) {
        const difference = val - this.nums[index];
        this.nums[index] = val;
        this._add(index + 1, difference);
    }
    
    sumRange(left, right) {
        // Compute the sum between left and right using prefix sums
        return this._prefixSum(right + 1) - this._prefixSum(left);
    }
}
```
**Time Complexity:**
- `update`: O(log n) time for each update operation.
- `sumRange`: O(log n) time for each query.
  
**Space Complexity:** O(n) additional space for maintaining the BIT.

### Segment Tree Approach

The Segment Tree is a more sophisticated data structure to efficiently handle queries on intervals or segments of an array and allows both update and sum operations to be performed efficiently.

#### Intuition
- Construct a tree where each node represents a segment of the array, and recursively build each node’s segment sum.
- During an update, traverse the tree to update relevant segments.
- For a range sum query, combine values from relevant segments to get the total sum.

```javascript
class NumArray {
    constructor(nums) {
        this.n = nums.length;
        this.tree = Array(2 * this.n).fill(0);
        
        // Build the segment tree
        for (let i = 0; i < this.n; i++) this.tree[this.n + i] = nums[i];
        for (let i = this.n - 1; i > 0; i--) this.tree[i] = this.tree[i * 2] + this.tree[i * 2 + 1];
    }
    
    update(index, val) {
        let pos = index + this.n;
        this.tree[pos] = val;
        
        // Update the tree upwards
        while (pos > 1) {
            pos >>= 1;
            this.tree[pos] = this.tree[pos * 2] + this.tree[pos * 2 + 1];
        }
    }
    
    sumRange(left, right) {
        let sum = 0;
      
        // Translate left and right to leaf positions in the tree
        left += this.n;
        right += this.n;
      
        while (left <= right) {
            if (left & 1) sum += this.tree[left++];
            if (!(right & 1)) sum += this.tree[right--];
            left >>= 1;
            right >>= 1;
        }
      
        return sum;
    }
}
```
**Time Complexity:**
- `update`: O(log n) time for each update operation.
- `sumRange`: O(log n) time for each query.
  
**Space Complexity:** O(n) for the segment tree. 

In conclusion, while the brute force approach is simple, it is inefficient for large inputs. The Fenwick Tree or Segment Tree offers significant improvements, enabling logarithmic time updates and queries, making them ideal for scalable solutions.

