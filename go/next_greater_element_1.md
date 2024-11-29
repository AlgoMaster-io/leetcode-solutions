# 496. [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n * m)
// Space Complexity: O(1)
func nextGreaterElement(nums1 []int, nums2 []int) []int {
    result := make([]int, len(nums1))

    // Iterate over each element in nums1
    for i, current := range nums1 {
        indexInNums2 := -1

        // Find the index of current in nums2
        for j := 0; j < len(nums2); j++ {
            if nums2[j] == current {
                indexInNums2 = j
                break
            }
        }

        // Look for the next greater element to the right in nums2
        result[i] = -1 // Default if no greater element is found
        for j := indexInNums2 + 1; j < len(nums2); j++ {
            if nums2[j] > current {
                result[i] = nums2[j]
                break
            }
        }
    }

    return result
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
go
```go
// Time Complexity: O(n + m)
// Space Complexity: O(m)
func nextGreaterElement(nums1 []int, nums2 []int) []int {
    nextGreaterMap := make(map[int]int)
    stack := []int{}

    // Traverse nums2 in reverse to populate the nextGreaterMap
    for _, num := range nums2 {
        // Maintain the stack in decreasing order
        for len(stack) > 0 && stack[len(stack)-1] <= num {
            stack = stack[:len(stack)-1]
        }
        // If stack is not empty, the top of the stack is the next greater element
        if len(stack) == 0 {
            nextGreaterMap[num] = -1
        } else {
            nextGreaterMap[num] = stack[len(stack)-1]
        }
        stack = append(stack, num)
    }

    // Build the result for nums1 using the map
    result := make([]int, len(nums1))
    for i, num := range nums1 {
        result[i] = nextGreaterMap[num]
    }

    return result
}
```

