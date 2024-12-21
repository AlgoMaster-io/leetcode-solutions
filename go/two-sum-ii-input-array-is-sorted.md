## [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointers](#approach-2-two-pointers)

### Approach 1: Brute Force

#### Intuition

Given that the array is sorted, the most straightforward way to solve the problem is to check every possible pair of numbers to see if they add up to the target.

#### Approach

1. Iterate over the array with the first pointer `i`.
2. For each element at index `i`, use a second pointer `j` to check every next element.
3. If the sum of the elements at indices `i` and `j` matches the target, return the indices (plus one to convert to the expected one-indexed solution).

#### Code

```go
func twoSum(numbers []int, target int) []int {
    n := len(numbers)
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            // Check if the current pair adds up to the target
            if numbers[i] + numbers[j] == target {
                return []int{i + 1, j + 1}  // convert to 1-based index
            }
        }
    }
    return nil  // return nil if no solution is found
}
```

#### Time Complexity
- **O(n^2)**: We are checking every pair of numbers.

#### Space Complexity
- **O(1)**: No extra space is used apart from variables.

### Approach 2: Two Pointers

#### Intuition

Given the sorted nature of the array, we can use a two-pointer approach to efficiently find the pair. By initializing one pointer at the start and the other at the end of the array, we can adjust the pointers based on the sum of the elements at these pointers compared to the target.

#### Approach

1. Initialize two pointers: `left` at the start (index 0) and `right` at the end (index `n-1`) of the array.
2. Calculate the sum of the elements at the `left` and `right` pointers.
3. If the sum equals the target, return the indices (plus one).
4. If the sum is less than the target, increment the `left` pointer to increase the sum.
5. If the sum is greater than the target, decrement the `right` pointer to decrease the sum.
6. Repeat until the pointers cross or a solution is found.

#### Code

```go
func twoSum(numbers []int, target int) []int {
    left, right := 0, len(numbers) - 1

    for left < right {
        sum := numbers[left] + numbers[right]

        if sum == target {
            // Return the indices as 1-based
            return []int{left + 1, right + 1}
        } else if sum < target {
            left++  // Move left pointer to increase sum
        } else {
            right--  // Move right pointer to decrease sum
        }
    }

    return nil  // return nil if no solution is found
}
```

#### Time Complexity
- **O(n)**: We traverse the array with two pointers, each step reducing the search space.

#### Space Complexity
- **O(1)**: No additional space other than variables is used.

