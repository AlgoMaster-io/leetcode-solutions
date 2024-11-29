# 307. [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity for update: O(1)
// Time Complexity for sumRange: O(n)
// Space Complexity: O(n)
type NumArray struct {
    nums []int
}

func Constructor(nums []int) NumArray {
    return NumArray{nums: nums}
}

func (this *NumArray) Update(index int, val int) {
    this.nums[index] = val
}

func (this *NumArray) SumRange(i int, j int) int {
    sum := 0
    for k := i; k <= j; k++ {
        sum += this.nums[k]
    }
    return sum
}
```

## Approach 2: Segment Tree

### Solution
go
```go
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
type NumArray struct {
    nums        []int
    segmentTree []int
}

func Constructor(nums []int) NumArray {
    n := len(nums)
    segmentTree := make([]int, 4*n)
    numArray := NumArray{nums: nums, segmentTree: segmentTree}
    numArray.buildTree(0, 0, n-1)
    return numArray
}

func (this *NumArray) buildTree(treeIndex, left, right int) {
    if left == right {
        this.segmentTree[treeIndex] = this.nums[left]
        return
    }
    mid := left + (right-left)/2
    leftChild := 2*treeIndex + 1
    rightChild := 2*treeIndex + 2
    this.buildTree(leftChild, left, mid)
    this.buildTree(rightChild, mid+1, right)

    this.segmentTree[treeIndex] = this.segmentTree[leftChild] + this.segmentTree[rightChild]
}

func (this *NumArray) Update(index int, val int) {
    this.updateRecursive(0, 0, len(this.nums)-1, index, val)
}

func (this *NumArray) updateRecursive(treeIndex, left, right, index, val int) {
    if left == right {
        this.segmentTree[treeIndex] = val
        this.nums[index] = val
        return
    }
    mid := left + (right-left)/2
    leftChild := 2*treeIndex + 1
    rightChild := 2*treeIndex + 2
    if index <= mid {
        this.updateRecursive(leftChild, left, mid, index, val)
    } else {
        this.updateRecursive(rightChild, mid+1, right, index, val)
    }
    this.segmentTree[treeIndex] = this.segmentTree[leftChild] + this.segmentTree[rightChild]
}

func (this *NumArray) SumRange(i int, j int) int {
    return this.sumRangeRecursive(0, 0, len(this.nums)-1, i, j)
}

func (this *NumArray) sumRangeRecursive(treeIndex, left, right, i, j int) int {
    if left > j || right < i {
        return 0
    }
    if i <= left && right <= j {
        return this.segmentTree[treeIndex]
    }
    mid := left + (right-left)/2
    leftChild := 2*treeIndex + 1
    rightChild := 2*treeIndex + 2
    return this.sumRangeRecursive(leftChild, left, mid, i, j) + this.sumRangeRecursive(rightChild, mid+1, right, i, j)
}
```

## Approach 3: Binary Indexed Tree (Fenwick Tree)

### Solution
go
```go
// Time Complexity for update: O(log n)
// Time Complexity for sumRange: O(log n)
// Space Complexity: O(n)
type NumArray struct {
    nums []int
    bit  []int
    n    int
}

func Constructor(nums []int) NumArray {
    n := len(nums)
    bit := make([]int, n+1)
    numArray := NumArray{nums: nums, bit: bit, n: n}
    for i, val := range nums {
        numArray.init(i, val)
    }
    return numArray
}

func (this *NumArray) init(index, val int) {
    index++
    for index <= this.n {
        this.bit[index] += val
        index += index & -index
    }
}

func (this *NumArray) Update(index int, val int) {
    delta := val - this.nums[index]
    this.nums[index] = val
    this.init(index, delta)
}

func (this *NumArray) getSum(index int) int {
    sum := 0
    index++
    for index > 0 {
        sum += this.bit[index]
        index -= index & -index
    }
    return sum
}

func (this *NumArray) SumRange(i int, j int) int {
    return this.getSum(j) - this.getSum(i-1)
}
```

