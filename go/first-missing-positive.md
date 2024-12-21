# [Leetcode 41: First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

## Approaches

- [Approach 1: Sort and Scan](#approach-1-sort-and-scan)
- [Approach 2: HashSet](#approach-2-hashset)
- [Approach 3: Cycle Sort (Optimal)](#approach-3-cycle-sort-optimal)

### Approach 1: Sort and Scan

**Intuition**:  
If we sort the array, the first missing positive number will be the first number in the sorted array sequence that breaks the series of consecutive positive numbers starting from 1. 

**Steps**:
1. Sort the array.
2. Initialize a variable `missing` to 1.
3. Iterate through the sorted array, and whenever you find a number equal to `missing`, increment `missing` by 1.
4. The first number that doesn't match `missing` when traversing the array is the answer.

```go
func firstMissingPositive(nums []int) int {
    sort.Ints(nums)  // Sort the numbers
    
    missing := 1     // Start by assuming the first missing positive is 1
    for _, num := range nums {
        // Only interested in positive numbers, as they might be candidates
        if num == missing {
            missing++  // Found the current missing number, increment
        }
    }
    return missing  // Return the first missing positive
}
```

- **Time Complexity**: O(n log n) due to sorting.
- **Space Complexity**: O(1) if sorting in-place, otherwise O(n).

### Approach 2: HashSet

**Intuition**:  
By leveraging a hash set, we can check in constant time whether a number exists. Thus, by iterating over numbers starting from 1, we can find the first missing positive efficiently.

**Steps**:
1. Place all the numbers in a hash set.
2. Start checking from 1 upwards to see which number is missing from the set.

```go
func firstMissingPositive(nums []int) int {
    numSet := make(map[int]struct{})
    for _, num := range nums {
        numSet[num] = struct{}{}
    }
    
    missing := 1  // Start from 1
    for {
        if _, exists := numSet[missing]; !exists {
            return missing  // If missing number is not in set, return it
        }
        missing++  // Increment to check next number
    }
}
```

- **Time Complexity**: O(n), assuming average O(1) time complexity for hash set operations.
- **Space Complexity**: O(n) for storing numbers in the set.

### Approach 3: Cycle Sort (Optimal)

**Intuition**:  
The idea is to place each positive number `nums[i]` >= 1 to the index `nums[i] - 1`, trying to place all numbers in their "correct" indices. Once placement is correct, the first index `i` which does not have `i+1` will be the missing positive number.

**Steps**:
1. Iterate through the array and position each positive number `x` at index `x - 1`.
2. Ignore numbers out of range or already positioned correctly.
3. After positioning, scan through the array to find the first position `i` not containing `i+1`.

```go
func firstMissingPositive(nums []int) int {
    n := len(nums)
    
    for i := 0; i < n; {
        // Since the position for number nums[i] is nums[i]-1, we check and swap if needed
        correctPos := nums[i] - 1
        if nums[i] >= 1 && nums[i] <= n && nums[i] != nums[correctPos] {
            nums[i], nums[correctPos] = nums[correctPos], nums[i]
        } else {
            i++  // If not swappable, proceed to the next index
        }
    }
    
    // After rearrangement, check which is the first index missing the correct number
    for i := 0; i < n; i++ {
        if nums[i] != i+1 {
            return i+1  // First index that does not have `i+1`
        }
    }
    
    // If all numbers are placed correctly, the missing number is n+1
    return n+1
}
```

- **Time Complexity**: O(n), as each number will be placed exactly once.
- **Space Complexity**: O(1), in-place modifications without extra space.

