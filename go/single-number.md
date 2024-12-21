# [Leetcode 136: Single Number](https://leetcode.com/problems/single-number/)

## Approaches
1. [Using a HashMap](#using-a-hashmap)
2. [Sorting](#sorting)
3. [Using XOR Operator (Optimal)](#using-xor-operator-optimal)

---

### Using a HashMap

#### Intuition:
Use a hash map to keep track of the number of times each element appears in the array. The element which appears only once will be our answer.

#### Steps:
1. Iterate over each element in the array.
2. For each element, update a map to count its occurrences.
3. Iterate over the map to find the element with an occurrence of 1.

#### Time Complexity:
- O(n): We traverse the array twice, once to populate the hash map and once to find the single number.
  
#### Space Complexity:
- O(n): We use a hash map to store the count of elements.

```go
func singleNumber(nums []int) int {
    freq := make(map[int]int)
    
    // Count the frequency of each element
    for _, num := range nums {
        freq[num]++
    }
    
    // Find the element with a count of 1
    for num, count := range freq {
        if count == 1 {
            return num
        }
    }
    return -1 // This line should never be reached
}
```

---

### Sorting

#### Intuition:
By sorting the array, numbers that are not the single number will appear twice consecutively. So, we can compare elements in pairs.

#### Steps:
1. Sort the array.
2. Traverse the array and check pairs of elements. The element which does not match its pair is the single number.
3. Edge case: If no unmatched element is found in pairs, the last element is the single number.

#### Time Complexity:
- O(n log n): Sorting the array dominates the time complexity.
  
#### Space Complexity:
- O(1): If ignoring the space required for sorting, no additional data structures are used.

```go
import "sort"

func singleNumber(nums []int) int {
    sort.Ints(nums)
    
    // Check pairs of elements
    for i := 0; i < len(nums)-1; i += 2 {
        if nums[i] != nums[i+1] {
            return nums[i]
        }
    }
    
    // Single number is the last element if no other is found
    return nums[len(nums)-1]
}
```

---

### Using XOR Operator (Optimal)

#### Intuition:
Using the properties of XOR: 
- `a ⊕ a = 0` for any integer `a`. 
- `a ⊕ 0 = a`
- XOR is commutative and associative.

By XORing all elements together, elements appearing twice cancel themselves out, leaving only the single number.

#### Steps:
1. Initialize a variable `result` to 0.
2. XOR each element of the array with `result`.
3. The value in `result` after the loop ends is the single number.

#### Time Complexity:
- O(n): We traverse the array once.

#### Space Complexity:
- O(1): We use a constant amount of additional space.
  
```go
func singleNumber(nums []int) int {
    result := 0
    
    // XOR all elements together
    for _, num := range nums {
        result ^= num
    }
    
    // The final result is the single number
    return result
}
```
---

These solutions provide different approaches to solve the "Single Number" problem, ranging from straightforward use of data structures to optimal use of bit manipulation.

