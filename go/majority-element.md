# [Leetcode Problem 169: Majority Element](https://leetcode.com/problems/majority-element/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Hash Map](#approach-2-hash-map)
- [Approach 3: Sorting](#approach-3-sorting)
- [Approach 4: Boyer-Moore Voting Algorithm](#approach-4-boyer-moore-voting-algorithm)

## Approach 1: Brute Force

### Intuition
The simplest approach is to count the frequency of each element and determine which has the majority count (appears more than `n/2` times in the array). We can do this by iterating over the array with nested loops.

### Implementation
```go
func majorityElement(nums []int) int {
    n := len(nums)
    for i := 0; i < n; i++ {
        count := 0
        for j := 0; j < n; j++ {
            if nums[j] == nums[i] {
                count++
            }
        }
        // If count of current element is more than half of array length
        if count > n/2 {
            return nums[i]
        }
    }
    return -1 // This line will never be reached as per the problem's guarantee
}
```

### Complexity Analysis
- **Time Complexity**: O(n^2), where n is the number of elements in the array, because of the nested loops.
- **Space Complexity**: O(1), since no extra space is used except for variables.

## Approach 2: Hash Map

### Intuition
Instead of checking each element repeatedly, we can store the count of each element in a hash map and then determine which element exceeds n/2 counts.

### Implementation
```go
func majorityElement(nums []int) int {
    countMap := make(map[int]int)
    n := len(nums)
    
    for _, num := range nums {
        countMap[num] += 1
        // Check if current element's count is more than half
        if countMap[num] > n/2 {
            return num
        }
    }
    return -1 // This line will never be reached as per the problem's guarantee
}
```

### Complexity Analysis
- **Time Complexity**: O(n), because we make a single pass through the array.
- **Space Complexity**: O(n), as we store counts of elements in a hash map.

## Approach 3: Sorting

### Intuition
By sorting the array, the majority element will always be located at the middle index (n/2). This is because more than half of the elements are identical.

### Implementation
```go
import "sort"

func majorityElement(nums []int) int {
    sort.Ints(nums)
    // The majority element must be at n/2 index after sorting
    return nums[len(nums)/2]
}
```

### Complexity Analysis
- **Time Complexity**: O(n log n), because of the sorting step.
- **Space Complexity**: O(1), as sorting is performed in-place.

## Approach 4: Boyer-Moore Voting Algorithm

### Intuition
The Boyer-Moore Voting Algorithm is a clever method that works as follows:
1. We maintain a `candidate` for majority element and a `count`, both initialized to zero.
2. Traverse the array. If the `count` is zero, we set the `candidate` to the current element and increase the `count`.
3. If the current element is the `candidate`, we increment the `count`. If it's not, we decrement the `count`.
4. The valid candidate at the end will be the majority element.

### Implementation
```go
func majorityElement(nums []int) int {
    candidate, count := 0, 0
    
    for _, num := range nums {
        if count == 0 {
            candidate = num
        }
        if num == candidate {
            count++
        } else {
            count--
        }
    }
    return candidate
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of elements in the array, since we pass through the array once.
- **Space Complexity**: O(1), as no additional data structures are used.

