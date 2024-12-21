# [Leetcode 189: Rotate Array](https://leetcode.com/problems/rotate-array/)

## Solutions:
1. [Brute Force Approach](#brute-force-approach)
2. [Using Extra Array](#using-extra-array)
3. [Cyclic Replacements](#cyclic-replacements)
4. [Reverse Array Method](#reverse-array-method)

---

### Brute Force Approach

**Intuition**:  
Rotate the array one element at a time up to `k` times. For each rotation, take the last element of the array and place it at the start, then shift all other elements to the right. This method is straightforward but inefficient for larger arrays.

**Algorithm**:
1. Repeat step 2 for `k` times.
2. Store the last element in a temporary variable.
3. Move all elements one position to the right.
4. Place the last element stored in the temporary variable in the first position.

```go
func rotate(nums []int, k int) {
    n := len(nums)
    k = k % n // In case k is greater than n
    for i := 0; i < k; i++ {
        last := nums[n-1] // Store the last element
        for j := n - 1; j > 0; j-- {
            nums[j] = nums[j-1] // Shift elements to the right
        }
        nums[0] = last // Place last element in the first position
    }
}
```
**Time Complexity**: O(n * k), where `n` is the number of elements in the array.  
**Space Complexity**: O(1), no extra space is used.

---

### Using Extra Array

**Intuition**:  
Create a new array to hold the final rotated array. Calculate the new position of each element and place it in the new array, and finally copy the elements back to the original array.

**Algorithm**:
1. Create a new array of the same size as the original.
2. For each element at index `i`, calculate the new index as `(i + k) % n`.
3. Copy the contents of the new array back to the original array.

```go
func rotate(nums []int, k int) {
    n := len(nums)
    k = k % n
    newArr := make([]int, n)
    for i := 0; i < n; i++ {
        newArr[(i+k)%n] = nums[i] // Calculate new index and place the element
    }
    copy(nums, newArr) // Copy newArr back to nums
}
```
**Time Complexity**: O(n), where `n` is the number of elements in the array.  
**Space Complexity**: O(n), due to the extra array used.

---

### Cyclic Replacements

**Intuition**:  
This method involves cyclic replacements, where you place each number in its correct position. Keep track of how many elements have been placed. This approach avoids additional storage.

**Algorithm**:
1. Iterate through the array, starting from each unplaced element.
2. Keep swapping elements in their correct positions `(current_index + k) % n`.
3. Keep track of the number of elements placed to stop when all are placed.

```go
func rotate(nums []int, k int) {
    n := len(nums)
    k = k % n
    count := 0
    for start := 0; count < n; start++ {
        current, prev := start, nums[start]
        for {
            next := (current + k) % n
            nums[next], prev = prev, nums[next]
            current = next
            count++
            if start == current {
                break
            }
        }
    }
}
```
**Time Complexity**: O(n), where `n` is the number of elements in the array.  
**Space Complexity**: O(1), no extra space is used.

---

### Reverse Array Method

**Intuition**:  
Reverse the entire array, then reverse the first `k` elements to bring the rotated portion to the front, finally reverse the remaining `n-k` elements to restore their original order. This approach is efficient and uses the least amount of space.

**Algorithm**:
1. Reverse the whole array.
2. Reverse the first `k` elements.
3. Reverse the remaining elements from `k` to end of the array.

```go
func rotate(nums []int, k int) {
    n := len(nums)
    k = k % n

    // Helper function to reverse a portion of the array
    reverse := func(start, end int) {
        for start < end {
            nums[start], nums[end] = nums[end], nums[start]
            start++
            end--
        }
    }

    reverse(0, n-1)   // Reverse the full array
    reverse(0, k-1)   // Reverse first k elements
    reverse(k, n-1)   // Reverse remaining elements
}
```
**Time Complexity**: O(n), where `n` is the number of elements in the array.  
**Space Complexity**: O(1), no extra space is used except for the in-place reversals.

