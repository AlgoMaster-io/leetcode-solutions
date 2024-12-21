# Leetcode Problem: [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two-Pass Approach with Extra Space](#approach-2-two-pass-approach-with-extra-space)
- [Approach 3: Two-Pointer Approach (Optimal)](#approach-3-two-pointer-approach-optimal)

### Approach 1: Brute Force

**Intuition:**
The simplest approach is to iterate over the array and create a new array where non-zero elements are copied first followed by the zeroes. This is straightforward but requires additional space which could be avoided.

**Time Complexity:** O(n), where n is the number of elements in the array.

**Space Complexity:** O(n), as we are using an extra array to store the result temporarily.

```go
func moveZeroes(nums []int) {
    // Create a new slice to hold non-zero elements
	result := make([]int, 0, len(nums))
	
    // First add all non-zero elements to the result slice
	for _, num := range nums {
		if num != 0 {
			result = append(result, num)
		}
	}
    
    // Add zeros to the end of the result slice
	for len(result) < len(nums) {
		result = append(result, 0)
	}
    
    // Copy the result slice back to the original nums slice
	copy(nums, result)
}
```

### Approach 2: Two-Pass Approach with Extra Space

**Intuition:**
This approach involves counting non-zero elements and moving them into the front of the array. Then fill the rest of the array with zeroes. This is more space-efficient than Approach 1 since we're directly modifying the array without using a new one.

**Time Complexity:** O(n), where n is the number of elements in the array.

**Space Complexity:** O(1), as we are modifying the array in place with minimal additional space.

```go
func moveZeroes(nums []int) {
    index := 0  // Initialize index for the placement of non-zero elements

	// First pass: Place all non-zero elements at the start of the array
    for _, num := range nums {
        // If the current element is non-zero, move it to the front
        if num != 0 {
            nums[index] = num
            index++
        }
    }
    
    // Second pass: Fill the rest of the array with zeroes
    for index < len(nums) {
        nums[index] = 0
        index++
    }
}
```

### Approach 3: Two-Pointer Approach (Optimal)

**Intuition:**
Using two pointers, one iterates over the array while the other keeps track of the position where the next non-zero element should be placed. If we encounter a non-zero element, we swap it with the element at the second pointer's position; this effectively moves all zeroes to the end.

**Time Complexity:** O(n), where n is the number of elements in the array.

**Space Complexity:** O(1), as we are modifying the array in place.

```go
func moveZeroes(nums []int) {
    lastNonZeroFoundAt := 0  // Pointer to track the position to place the next non-zero element

    // Iterate over all elements in the array
    for current := 0; current < len(nums); current++ {
        // If the current element is non-zero, swap it with the element at lastNonZeroFoundAt
        if nums[current] != 0 {
            nums[lastNonZeroFoundAt], nums[current] = nums[current], nums[lastNonZeroFoundAt]
            lastNonZeroFoundAt++  // Move the lastNonZeroFoundAt pointer to the next position
        }
    }
}
```

Each approach refines the algorithm to be more efficient, moving from a space-intensive solution to an optimal in-place solution using two pointers.

