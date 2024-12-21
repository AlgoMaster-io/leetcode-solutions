[Leetcode Problem 26: Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Two Pointers Approach](#two-pointers-approach)

---

### Brute Force Approach

#### Intuition
A straightforward way to approach this problem is to iterate through the array and repeatedly remove the duplicate elements by creating a new slice and copying unique elements over. While intuitive, this is not optimal in terms of time complexity because it involves repeated copying of elements.

#### Steps
1. Use a separate slice to store unique elements. 
2. Iterate through the array, checking if the current element is the same as the last one added to the slice.
3. If not, append it to the slice of unique elements.
4. Update the original array with these unique elements at the beginning.

#### Code
```go
func removeDuplicates(nums []int) int {
    if len(nums) == 0 {
        return 0
    }

    uniqueSlice := []int{nums[0]}
    
    for i := 1; i < len(nums); i++ {
        // Check if the current element is not the same as the last element in the unique slice
        if nums[i] != uniqueSlice[len(uniqueSlice)-1] {
            // Append it to uniqueSlice
            uniqueSlice = append(uniqueSlice, nums[i])
        }
    }

    // Copy elements back to the beginning of the original array
    for i := 0; i < len(uniqueSlice); i++ {
        nums[i] = uniqueSlice[i]
    }

    return len(uniqueSlice)
}
```

#### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of elements in the array. Each element is processed once.
- **Space Complexity:** O(n), due to the use of an additional data structure to store unique elements.

---

### Two Pointers Approach

#### Intuition
A more optimal approach uses two pointers to keep track of unique elements directly in the original array. By maintaining a slow pointer for the position of the next unique element and a fast pointer for exploring the array, we can update the array in-place without using extra space.

#### Steps
1. Use a "slow" pointer to track the position for the next unique element.
2. Use a "fast" pointer to iterate through the array:
   - If the current element is not equal to the element indicated by "slow", increment "slow" and update the position with the current element.
3. The index of the "slow" pointer + 1 at the end of iterations gives the length of the array without duplicates.

#### Code
```go
func removeDuplicates(nums []int) int {
    if len(nums) == 0 {
        return 0
    }

    j := 0  // slow pointer
    for i := 1; i < len(nums); i++ {
        // If current element (fast) is not equal to element at slow pointer
        if nums[i] != nums[j] {
            j++  // move slow pointer
            nums[j] = nums[i]  // update position with current unique element
        }
    }

    return j + 1
}
```

#### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of elements in the array. The list is traversed only once by the fast pointer.
- **Space Complexity:** O(1), as no additional space is used apart from variables.

Using the two pointers approach, the problem is solved both efficiently and in place.

