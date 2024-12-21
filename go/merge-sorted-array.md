# [Leetcode 88: Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Approaches
1. [Two Pointers from Start (Non-optimal)](#approach-1)
2. [Two Pointers from End (Optimal)](#approach-2)

---

## Approach 1: Two Pointers from Start (Non-optimal)

### Intuition
The basic idea is to use two pointers, one for each array, and compare elements from `nums1` and `nums2`. We select the smaller element of the two pointed by the pointers into a new temporary array. However, this approach is not space-efficient because it requires additional space for the temporary array.

### Steps:
1. Initialize two pointers, `p1` and `p2`, both starting at index 0 for `nums1` and `nums2` respectively.
2. Create a new array `temp` to store the result.
3. While both pointers have not exceeded the lengths of their respective arrays, compare `nums1[p1]` and `nums2[p2]`.
4. Append the smaller element to `temp` and move the corresponding pointer forward.
5. When one of the arrays is exhausted, append the remaining elements from the other array to `temp`.
6. Finally, copy `temp` back into `nums1`.

### Code

```go
func merge(nums1 []int, m int, nums2 []int, n int) {
    // Create a temporary array to store the merged result
    temp := make([]int, 0, m+n)
    
    i, j := 0, 0
    
    // Merge elements from nums1 and nums2
    for i < m && j < n {
        if nums1[i] < nums2[j] {
            temp = append(temp, nums1[i])
            i++
        } else {
            temp = append(temp, nums2[j])
            j++
        }
    }
    
    // If there are remaining elements in nums1, add them to temp
    for i < m {
        temp = append(temp, nums1[i])
        i++
    }
    
    // If there are remaining elements in nums2, add them to temp
    for j < n {
        temp = append(temp, nums2[j])
        j++
    }
    
    // Copy the elements from temp back to nums1
    for i := 0; i < m+n; i++ {
        nums1[i] = temp[i]
    }
}
```

### Complexity
- **Time Complexity:** O(m + n), where m is the number of initialized elements in `nums1` and n is the number of elements in `nums2`.
- **Space Complexity:** O(m + n) due to the use of additional array `temp`.

---

## Approach 2: Two Pointers from End (Optimal)

### Intuition
Instead of a separate array, we can use the end of `nums1` to place the merged elements. This allows us to perform the merge in place, which is space-efficient. We'll start merging from the back to avoid overwriting values in `nums1`.

### Steps:
1. Initialize a pointer `p` at the end of the merger space in `nums1`, `p1` at the end of the initialized elements in `nums1`, and `p2` at the end of `nums2`.
2. Compare elements pointed by `p1` and `p2` and place the larger element at `p`.
3. Move the pointers accordingly:
   - If `nums1[p1]` is larger, decrease `p1`; otherwise, decrease `p2`.
   - Always decrease `p`.
4. If there are remaining elements in `nums2`, fill them in `nums1` as they will all be smaller than the elements already considered from `nums1`.

### Code

```go
func merge(nums1 []int, m int, nums2 []int, n int) {
    // Pointers for nums1 and nums2
    p1, p2 := m-1, n-1

    // Pointer for the last index of nums1
    p := m + n - 1

    // Compare elements from nums1 and nums2 and fill nums1 from the end
    for p1 >= 0 && p2 >= 0 {
        if nums1[p1] > nums2[p2] {
            nums1[p] = nums1[p1]
            p1--
        } else {
            nums1[p] = nums2[p2]
            p2--
        }
        p--
    }

    // Fill nums1 with leftover nums2 elements if any
    for p2 >= 0 {
        nums1[p] = nums2[p2]
        p2--
        p--
    }
}
```

### Complexity
- **Time Complexity:** O(m + n), where m is the number of initialized elements in `nums1` and n is the number of elements in `nums2`.
- **Space Complexity:** O(1) since we are modifying `nums1` in place without additional data structures.

