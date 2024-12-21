# [Leetcode 307: Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approaches:
1. [Naive Approach](#naive-approach)
2. [Segment Tree Approach](#segment-tree-approach)
3. [Binary Indexed Tree (Fenwick Tree) Approach](#binary-indexed-tree-approach)

### Naive Approach

The most straightforward way to solve this problem is to simply update the array as requested and perform a sum operation by iterating over the specified range.

#### Intuition:
- For each `update` operation, simply change the value at the specified index in the array.
- For each `sumRange` query, iterate from the start of the range to the end and calculate the sum.

#### Code:

```go
type NumArray struct {
    nums []int
}

func Constructor(nums []int) NumArray {
    return NumArray{nums: nums}
}

func (na *NumArray) Update(index int, val int) {
    na.nums[index] = val
}

func (na *NumArray) SumRange(left int, right int) int {
    sum := 0
    for i := left; i <= right; i++ {
        sum += na.nums[i]
    }
    return sum
}
```

#### Complexity Analysis:
- **Time Complexity**:
  - `Update`: O(1) time.
  - `SumRange`: O(n) in the worst case, where n is the number of elements in the range `[left, right]`.
- **Space Complexity**: O(1), excluding the input data.

### Segment Tree Approach

To handle range queries and updates more efficiently, we can use a segment tree.

#### Intuition:
- A segment tree allows querying and updating ranges in logarithmic time.
- We build the tree such that each node represents the sum of a segment of the array.
- Updates and range queries can be performed by traversing the tree and updating/querying relevant nodes.

#### Code:

```go
type NumArray struct {
    nums []int
    segTree []int
    n int
}

func Constructor(nums []int) NumArray {
    n := len(nums)
    segTree := make([]int, 4*n)
    na := NumArray{nums: nums, segTree: segTree, n: n}
    na.build(0, 0, n-1)
    return na
}

func (na *NumArray) build(node, start, end int) {
    if start == end {
        na.segTree[node] = na.nums[start]
    } else {
        mid := (start + end) / 2
        leftChild := 2*node + 1
        rightChild := 2*node + 2
        na.build(leftChild, start, mid)
        na.build(rightChild, mid+1, end)
        na.segTree[node] = na.segTree[leftChild] + na.segTree[rightChild]
    }
}

func (na *NumArray) Update(index int, val int) {
    na.updateTree(0, 0, na.n-1, index, val)
}

func (na *NumArray) updateTree(node, start, end, idx, val int) {
    if start == end {
        na.nums[idx] = val
        na.segTree[node] = val
    } else {
        mid := (start + end) / 2
        leftChild := 2*node + 1
        rightChild := 2*node + 2
        if start <= idx && idx <= mid {
            na.updateTree(leftChild, start, mid, idx, val)
        } else {
            na.updateTree(rightChild, mid+1, end, idx, val)
        }
        na.segTree[node] = na.segTree[leftChild] + na.segTree[rightChild]
    }
}

func (na *NumArray) SumRange(left int, right int) int {
    return na.queryTree(0, 0, na.n-1, left, right)
}

func (na *NumArray) queryTree(node, start, end, L, R int) int {
    if R < start || end < L {
        return 0
    }
    if L <= start && end <= R {
        return na.segTree[node]
    }
    mid := (start + end) / 2
    leftSum := na.queryTree(2*node+1, start, mid, L, R)
    rightSum := na.queryTree(2*node+2, mid+1, end, L, R)
    return leftSum + rightSum
}
```

#### Complexity Analysis:
- **Time Complexity**:
  - `Update` and `SumRange`: O(log n), since we are traversing the tree.
- **Space Complexity**: O(n) due to the storage of the segment tree.

### Binary Indexed Tree (Fenwick Tree) Approach

The Binary Indexed Tree or Fenwick Tree is another data structure that allows logarithmic range queries and updates.

#### Intuition:
- Like the segment tree, a Binary Indexed Tree allows efficient range queries.
- We maintain an auxiliary array `BIT` where we can efficiently update and query cumulative frequency.

#### Code:

```go
type NumArray struct {
    nums []int
    bit []int
    n int
}

func Constructor(nums []int) NumArray {
    n := len(nums)
    bit := make([]int, n+1)
    na := NumArray{nums: nums, bit: bit, n: n}
    for i := range nums {
        na.add(i+1, nums[i])
    }
    return na
}

func (na *NumArray) add(index, val int) {
    for index <= na.n {
        na.bit[index] += val
        index += index & (-index)
    }
}

func (na *NumArray) Update(index int, val int) {
    diff := val - na.nums[index]
    na.nums[index] = val
    na.add(index+1, diff)
}

func (na *NumArray) SumRange(left int, right int) int {
    return na.getPrefixSum(right+1) - na.getPrefixSum(left)
}

func (na *NumArray) getPrefixSum(index int) int {
    sum := 0
    for index > 0 {
        sum += na.bit[index]
        index -= index & (-index)
    }
    return sum
}
```

#### Complexity Analysis:
- **Time Complexity**:
  - `Update` and `SumRange`: O(log n) due to traversal of the tree structure.
- **Space Complexity**: O(n) for storing the BIT array.

These solutions start from a simple one and move to more optimal ones, implementing efficient data structures to handle the mutable range sum query.

