# [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Approach 1: Binary Search for the Complement

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(1)
func twoSum(numbers []int, target int) []int {
    for i := 0; i < len(numbers); i++ {
        complement := target - numbers[i]
        index := binarySearch(numbers, i+1, len(numbers)-1, complement)

        if index != -1 {
            return []int{i + 1, index + 1} // 1-based index
        }
    }

    return []int{-1, -1} // No solution found
}

func binarySearch(numbers []int, left, right, target int) int {
    for left <= right {
        mid := left + (right-left)/2
        if numbers[mid] == target {
            return mid
        } else if numbers[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return -1 // Target not found
}
```

## Approach 2: HashMap (For Non-Sorted Input)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
func twoSum(numbers []int, target int) []int {
    hashMap := make(map[int]int)

    for i, number := range numbers {
        complement := target - number

        if index, found := hashMap[complement]; found {
            return []int{index + 1, i + 1} // 1-based index
        }

        hashMap[number] = i
    }

    return []int{-1, -1} // No solution found
}
```

## Approach 3: Two Pointers

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func twoSum(numbers []int, target int) []int {
    left, right := 0, len(numbers)-1

    for left < right {
        sum := numbers[left] + numbers[right]

        if sum == target {
            return []int{left + 1, right + 1} // 1-based index
        } else if sum < target {
            left++ // Move left pointer to increase sum
        } else {
            right-- // Move right pointer to decrease sum
        }
    }

    return []int{-1, -1} // No solution found
}
```

