# [Leetcode 128: Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

## Approaches
- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: HashSet for constant time checks](#approach-2-hashset-for-constant-time-checks)
- [Approach 3: Optimized HashSet](#approach-3-optimized-hashset)

## Approach 1: Sorting

### Intuition
The basic approach to solve the problem is to sort the numbers. Once sorted, the consecutive numbers will be adjacent to each other, and the longest consecutive sequence can be found by traversing through the sorted list.

### Code

```go
func longestConsecutive(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    
    // Sort the numbers
    sort.Ints(nums)
    
    // Variables to keep track of the longest streak
    longestStreak := 1
    currentStreak := 1
    
    for i := 1; i < len(nums); i++ {
        // If the current number is the same as the previous number, skip it
        if nums[i] == nums[i-1] {
            continue
        }
        // Check if the current number is consecutive
        if nums[i] == nums[i-1]+1 {
            currentStreak++
        } else {
            longestStreak = max(longestStreak, currentStreak)
            currentStreak = 1
        }
    }
    
    return max(longestStreak, currentStreak)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity Analysis
- **Time complexity**: \(O(n \log n)\) due to the sorting step.
- **Space complexity**: \(O(1)\) or \(O(n)\) depending on sorting algorithm.

## Approach 2: HashSet for constant time checks

### Intuition
To optimize, we can use a HashSet to store the numbers. This allows O(1) average time complexity for checking if a number exists in the set. By iterating over the numbers and starting a consecutive count only from numbers that are potential sequence starts, we can find the longest consecutive sequence efficiently.

### Code

```go
func longestConsecutive(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    
    numSet := make(map[int]bool)
    // Populate the HashSet 
    for _, num := range nums {
        numSet[num] = true
    }
    
    longestStreak := 0
    
    for num := range numSet {
        // Check if the current number is a sequence start
        if !numSet[num-1] {
            currentNum := num
            currentStreak := 1
            
            // Increment the sequence
            for numSet[currentNum+1] {
                currentNum++
                currentStreak++
            }
            
            longestStreak = max(longestStreak, currentStreak)
        }
    }
    
    return longestStreak
}
```

### Complexity Analysis
- **Time complexity**: \(O(n)\) because each number is processed at most twice.
- **Space complexity**: \(O(n)\) for storing numbers in the set.

## Approach 3: Optimized HashSet

### Intuition
Similar to the previous HashSet approach, but we avoid setting values in HashSet multiple times. This solution is essentially the same as the previous one in terms of time complexity, but the explanation can help conceptual understanding in different ways.

### Code

```go
func longestConsecutive(nums []int) int {
    numSet := make(map[int]bool)
    
    for _, num := range nums {
        numSet[num] = true
    }
    
    longestStreak := 0
    
    for num := range numSet {
        if _, found := numSet[num-1]; !found {
            currentNum := num
            currentStreak := 1
            
            for {
                if _, exists := numSet[currentNum+1]; exists {
                    currentNum++
                    currentStreak++
                } else {
                    break
                }
            }
            
            longestStreak = max(longestStreak, currentStreak)
        }
    }
    
    return longestStreak
}
```

### Complexity Analysis
- **Time complexity**: \(O(n)\) as each number in the set is visited and processed once.
- **Space complexity**: \(O(n)\) for the storage of the HashSet.

