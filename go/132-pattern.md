[Leetcode 456: 132 Pattern](https://leetcode.com/problems/132-pattern/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two-pass Array Traversal](#approach-2-two-pass-array-traversal)
- [Approach 3: Monotonic Stack (Optimal)](#approach-3-monotonic-stack-optimal)

### Approach 1: Brute Force

The most basic approach is to directly try all possible triplets in the array. This means iterating over all possible `i, j, k` indices and checking if they satisfy the 132 pattern condition. This approach will help us understand the problem but is inefficient for large arrays.

**Intuition**:  
To find the 132 pattern, we need three indices `i < j < k` such that `nums[i] < nums[k] < nums[j]`. We can simply use three nested loops to check all combinations.

**Steps**:
1. Iterate over the array with the first loop pointing to `i`.
2. From `i + 1`, iterate for `j`.
3. From `j + 1`, iterate for `k`.
4. Check if the current indices satisfy the condition `nums[i] < nums[k] < nums[j]`.
5. Return true if found; false otherwise.

**Time Complexity**: O(n^3)  
**Space Complexity**: O(1)

```go
func find132pattern(nums []int) bool {
    n := len(nums)
    
    for i := 0; i < n-2; i++ {
        for j := i + 1; j < n-1; j++ {
            for k := j + 1; k < n; k++ {
                if nums[i] < nums[k] && nums[k] < nums[j] {
                    return true
                }
            }
        }
    }
    return false
}
```

### Approach 2: Two-pass Array Traversal

**Intuition**:  
We can hold the `nums[i]` fixed and try to find a suitable `nums[j] > nums[i]` and `nums[k]` such that `nums[k] > nums[i]` and is less than `nums[j]`. We maintain the minimum value until the current index with `min[i]` to keep track of potential `nums[i]`.

**Steps**:
1. Compute a `min` array where `min[i]` is the smallest value from the start to index `i`.
2. Iterate over possible `j` starting from 1 to n-1.
3. For each `j`, check each `k` from the end of the array for a possible `nums[k]`.
4. If `min[i] < nums[k] < nums[j]`, return true.

**Time Complexity**: O(n^2)  
**Space Complexity**: O(n)

```go
func find132pattern(nums []int) bool {
    n := len(nums)
    if n < 3 {
        return false
    }

    min := make([]int, n)
    min[0] = nums[0]

    for i := 1; i < n; i++ {
        min[i] = go(smallest value from 0 to i)
    }
    
    for j := 1; j < n-1; j++ {
        for k := j + 1; k < n; k++ {
            if min[j-1] < nums[k] && nums[k] < nums[j] {
                return true
            }
        }
    }
    return false
}
```
### Approach 3: Monotonic Stack (Optimal)

**Intuition**:  
The key insight is to check elements in a descending manner from the end. We maintain a stack where we keep track of potential `nums[k]` values, ensuring the top of the stack is always a potentially valid element for the 132 pattern. This exploits the monotonic properties to effectively reduce time complexity. 

**Steps**:
1. Traverse the list backward.
2. Maintain a possible value for `nums[k]` that satisfies `nums[i] < nums[k] < nums[j]`.
3. Use a stack to manage potential `nums[k]` values.
4. Use a variable `third` to store the largest valid `nums[k]` found so that when a candidate `nums[i]` (from backward) can compare.
5. If the current number is less than `third`, return true.

**Time Complexity**: O(n)  
**Space Complexity**: O(n)

```go
func find132pattern(nums []int) bool {
    n := len(nums)
    if n < 3 {
        return false
    }

    stack := []int{}
    third := int(^uint(0) >> 1) // Initialize to the smallest integer

    for i := n - 1; i >= 0; i-- {
        if nums[i] < third {
            return true
        }
        for len(stack) > 0 && nums[i] > stack[len(stack)-1] {
            third = stack[len(stack)-1]
            stack = stack[:len(stack)-1] // Pop the element
        }
        stack = append(stack, nums[i])
    }
    return false
}
```

