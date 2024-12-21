# [260. Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approaches:
1. [Hash Map Approach](#hash-map-approach)
2. [Bit Manipulation Approach](#bit-manipulation-approach)

---

## Hash Map Approach

### Intuition:
The problem is to find the two numbers that appear only once in an array where every other number appears exactly twice. A straightforward solution is to use a hash map to count the occurrences of each number and then identify the two that appear only once.

### Code:
```go
package main

import "fmt"

func singleNumber(nums []int) []int {
    // Create a map to hold the frequency of each number
    freq := make(map[int]int)

    // Count the occurrences of each number in the array
    for _, num := range nums {
        freq[num]++
    }

    // Iterate through the map to find numbers with frequency equal to 1
    res := []int{}
    for num, count := range freq {
        if count == 1 {
            res = append(res, num)
        }
    }

    return res
}

func main() {
    nums := []int{1, 2, 1, 3, 2, 5}
    fmt.Println(singleNumber(nums)) // Output: [3 5]
}
```

### Time Complexity:
- O(n), where n is the length of the input array, since we need to traverse the array to build the frequency map.

### Space Complexity:
- O(n), for storing the frequency map.

---

## Bit Manipulation Approach

### Intuition:
To solve this problem optimally, we use bit manipulation. The XOR of all numbers in the array will result in the XOR of the two unique numbers since XOR of a number with itself is zero and the XOR of zero with a number is the number itself.

To separate the two unique numbers, we find a set bit in the XOR result, which indicates different bits between the two numbers. Using this bit, we partition the numbers into two groups: one group where numbers have this bit set, and another where it is not set. Each group will contain one of the unique numbers, which can be found by XORing the numbers within the group.

### Code:
```go
package main

import "fmt"

func singleNumber(nums []int) []int {
    // XOR all the numbers to find the XOR of the two unique numbers
    xor := 0
    for _, num := range nums {
        xor ^= num
    }

    // Find a bit that is set in the XOR (different bits in unique numbers)
    diffBit := xor & (-xor)

    // Divide numbers into two groups and find the unique number in each
    x, y := 0, 0
    for _, num := range nums {
        // Use the bit to differentiate the two unique numbers
        if num & diffBit == 0 {
            x ^= num
        } else {
            y ^= num
        }
    }

    return []int{x, y}
}

func main() {
    nums := []int{1, 2, 1, 3, 2, 5}
    fmt.Println(singleNumber(nums)) // Output: [3 5]
}
```

### Time Complexity:
- O(n), where n is the length of the input array, since we loop through the array a constant number of times.

### Space Complexity:
- O(1), as no additional space is used other than a few variables.

