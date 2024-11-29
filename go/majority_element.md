# [169. Majority Element](https://leetcode.com/problems/majority-element/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
func majorityElement(nums []int) int {
    majorityCount := len(nums) / 2

    // Check the count of each element
    for _, num := range nums {
        count := 0
        for _, element := range nums {
            if element == num {
                count++
            }
        }
        // If count exceeds majority, return the element
        if count > majorityCount {
            return num
        }
    }

    return -1 // Shouldn't reach here as per problem constraints
}
```

## Approach 2: HashMap to Count Frequencies

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
func majorityElement(nums []int) int {
    countMap := make(map[int]int)
    majorityCount := len(nums) / 2

    // Count frequencies of elements
    for _, num := range nums {
        countMap[num] += 1

        // If an element's count exceeds majority, return it
        if countMap[num] > majorityCount {
            return num
        }
    }

    return -1 // Shouldn't reach here as per problem constraints
}
```

## Approach 3: Sorting

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(1)
import "sort"

func majorityElement(nums []int) int {
    // Sort the array
    sort.Ints(nums)

    // The majority element will always be at the middle index
    return nums[len(nums)/2]
}
```

## Approach 4: Boyer-Moore Voting Algorithm

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func majorityElement(nums []int) int {
    count := 0
    var candidate int

    // Find the candidate for the majority element
    for _, num := range nums {
        if count == 0 {
            candidate = num // Set a new candidate
        }
        if num == candidate {
            count++
        } else {
            count--
        }
    }

    return candidate // The problem guarantees the majority element exists
}
```


