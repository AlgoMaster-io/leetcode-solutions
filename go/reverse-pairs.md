# [LeetCode 493: Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Approaches
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Merge Sort Approach](#approach-2-merge-sort-approach)
3. [Fenwick Tree Approach](#approach-3-fenwick-tree-approach)

## Approach 1: Brute Force Approach

### Intuition
In the brute force approach, we check every pair of indices `(i, j)` such that `i < j` to see if `nums[i] > 2 * nums[j]`. This ensures that we cover all possible pairs to report the count of reverse pairs. However, this approach is not efficient for larger inputs, but it serves as a straightforward solution to solve the problem.

### Code
```go
func reversePairs(nums []int) int {
    n := len(nums)
    count := 0
    
    // Check each pair (i, j) to see if nums[i] > 2 * nums[j]
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            if nums[i] > 2 * nums[j] {
                count++
            }
        }
    }
    
    return count
}
```

### Complexity
- **Time Complexity:** O(n^2), where n is the length of the array. This is because we check each pair of indices.
- **Space Complexity:** O(1), no extra space is needed beyond a few variables.

## Approach 2: Merge Sort Approach

### Intuition
This method uses a modified merge sort algorithm to efficiently count the reverse pairs. The main concept is to leverage the divide-and-conquer strategy to both sort the elements and count the valid pairs during the merge process. Before merging two halves, we count the cross reverse pairs since a reverse pair `(i, j)` where `i < j` and `nums[i] > 2 * nums[j]` can only exist across different segments.

### Code
```go
func reversePairs(nums []int) int {
    if len(nums) < 2 {
        return 0
    }
    
    var count int
    
    var mergeSort func(int, int) []int
    mergeSort = func(left, right int) []int {
        if left == right {
            return []int{nums[left]}
        }
        
        mid := (left + right) / 2
        leftSorted := mergeSort(left, mid)
        rightSorted := mergeSort(mid+1, right)
        
        // Count reverse pairs across the two halves
        count += countReversePairs(leftSorted, rightSorted)
        
        return merge(leftSorted, rightSorted)
    }
    
    mergeSort(0, len(nums)-1)
    return count
}

func countReversePairs(leftSorted []int, rightSorted []int) int {
    count := 0
    j := 0
    
    // Count reverse pairs between left and right sorted arrays
    for _, val := range leftSorted {
        for j < len(rightSorted) && val > 2 * rightSorted[j] {
            j++
        }
        count += j
    }
    return count
}

func merge(left []int, right []int) []int {
    merged := make([]int, 0, len(left) + len(right))
    i, j := 0, 0
    
    for i < len(left) && j < len(right) {
        if left[i] <= right[j] {
            merged = append(merged, left[i])
            i++
        } else {
            merged = append(merged, right[j])
            j++
        }
    }
    
    merged = append(merged, left[i:]...)
    merged = append(merged, right[j:]...)
    
    return merged
}
```

### Complexity
- **Time Complexity:** O(n log n), because the algorithm is a modification of the merge sort.
- **Space Complexity:** O(n), as additional space is used for the temporary arrays during merging.

## Approach 3: Fenwick Tree Approach

### Intuition
Using a Fenwick Tree (or Binary Indexed Tree) allows us to efficiently count reverse pairs by iterating through the array and maintaining frequency counts of previously seen numbers. This solution leverages the property of Fenwick Trees for performing prefix sum queries and updates in logarithmic time. We first compress the values to better manage large data.

### Code
```go
func reversePairs(nums []int) int {
    sorted := make([]int, len(nums))
    copy(sorted, nums)
    sort.Ints(sorted)
    
    // Fenwick Tree Usage
    tree := make([]int, len(nums)+1)
    var update func(int, int)
    update = func(i, delta int) {
        for i > 0 && i < len(tree) {
            tree[i] += delta
            i += i & -i
        }
    }
    
    var query func(int) int
    query = func(i int) int {
        sum := 0
        for i > 0 {
            sum += tree[i]
            i -= i & -i
        }
        return sum
    }
    
    count := 0
    for _, x := range nums {
        // Count reverse pairs where nums[i] > 2 * x
        count += query(findIndex(2*x+1, sorted))
        
        // Update Fenwick Tree with current element
        update(findIndex(x, sorted), 1)
    }
    
    return count
}

func findIndex(x int, sorted []int) int {
    return sort.SearchInts(sorted, x) + 1
}
```

### Complexity
- **Time Complexity:** O(n log n), due to Fenwick Tree operations and sorting.
- **Space Complexity:** O(n), for storing the Fenwick Tree.

