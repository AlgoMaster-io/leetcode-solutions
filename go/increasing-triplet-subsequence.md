# [334. Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointers](#approach-2-two-pointers)

---

## Approach 1: Brute Force

### Intuition
The idea is to use a triple nested loop to check every possible combination of three numbers in the array. If we find such a combination that follows the condition `nums[i] < nums[j] < nums[k]`, we return `true`.

### Steps
1. Iterate over the array with three nested loops.
2. Check if for any combination `(i, j, k)`, the condition holds.
3. If such a triplet is found, return `true`.
4. If no such triplet is found, return `false`.

### Time and Space Complexity
- **Time Complexity**: O(n^3), where n is the number of elements in the array. Each element is examined in conjunction with every other pair.
- **Space Complexity**: O(1), no extra space is used aside from auxiliary variables.

### Go Code
```go
func increasingTriplet(nums []int) bool {
    n := len(nums)
    for i := 0; i < n; i++ { // First element
        for j := i + 1; j < n; j++ { // Second element greater than first
            if nums[j] > nums[i] {
                for k := j + 1; k < n; k++ { // Third element greater than second
                    if nums[k] > nums[j] {
                        return true // Found a valid triplet
                    }
                }
            }
        }
    }
    return false // No increasing triplet found
}
```

---

## Approach 2: Two Pointers

### Intuition
The goal is to find the smallest triplet indicative of an increasing subsequence using two variables to represent the smallest and second smallest values encountered so far. As we iterate through the array, update these variables accordingly. If we find a number greater than the second smallest, we have our increasing triplet.

### Steps
1. Initialize two variables `first` and `second` as the largest possible values.
2. Traverse each number in the array:
   - If the current number is less than or equal to `first`, update `first`.
   - Else if the current number is less than or equal to `second`, update `second`.
   - Else, the current number is greater than both `first` and `second`, indicating an increasing triplet.
3. Return `true` if such a triplet is found. Otherwise, `false`.

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the number of elements in the array.
- **Space Complexity**: O(1), only constant space is used for variables `first` and `second`.

### Go Code
```go
func increasingTriplet(nums []int) bool {
    first, second := int(^uint(0) >> 1), int(^uint(0) >> 1) // Initialize to max integer value

    for _, num := range nums {
        if num <= first {
            first = num // Update first if current number is smaller
        } else if num <= second {
            second = num // Update second if current number is between first and second
        } else {
            return true // Tripet found: num is greater than both first and second
        }
    }
    return false // No increasing triplet found
}
```


