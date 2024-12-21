# [Leetcode 421: Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)

## Approaches
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Bitwise Trie Approach](#approach-2-bitwise-trie-approach)

## Approach 1: Brute Force Approach

### Intuition
The brute force approach involves iterating through all pairs of numbers in the array and calculating their XOR. The goal is to find and return the maximum XOR value among all possible pairs.

### Detailed Steps
1. Loop over each element in the array with the first index `i`.
2. For each `i`, loop again with index `j` starting from `i+1` to avoid duplicate and self-pairing.
3. Calculate the XOR for each pair `(nums[i], nums[j])`.
4. Keep track of the maximum XOR value encountered.
5. Return the maximum XOR value after checking all pairs.

### Code
```go
package main

import "fmt"

func findMaximumXOR(nums []int) int {
    maxXOR := 0
    n := len(nums)
    for i := 0; i < n; i++ {
        for j := i+1; j < n; j++ {
            currentXOR := nums[i] ^ nums[j] // Calculate XOR for pair (nums[i], nums[j])
            if currentXOR > maxXOR {
                maxXOR = currentXOR // Update maxXOR if found a larger value
            }
        }
    }
    return maxXOR
}

func main() {
    nums := []int{3, 10, 5, 25, 2, 8}
    fmt.Println(findMaximumXOR(nums)) // Output: 28
}
```

### Complexity Analysis
- **Time Complexity**: \(O(n^2)\) - We are checking every pair of numbers in the array.
- **Space Complexity**: \(O(1)\) - No extra space is used beyond a few variable assignments.

## Approach 2: Bitwise Trie Approach

### Intuition
To optimize the solution, we use a bitwise trie (prefix tree). By storing binary representations of numbers and trying to maximize XOR bit-by-bit, we can determine the best complementary number for maximizing XOR.

### Detailed Steps
1. Insert all numbers' binary representation into a Trie, starting with the most significant bit.
2. For each number, try to build the number that gives the maximum XOR by traversing the Trie.
3. At every bit position, decide whether to branch left (0) or right (1) based on the bit value to achieve the maximal result.

### Code
```go
package main

import "fmt"

// TrieNode representing a bit in a binary Trie
type TrieNode struct {
    children [2]*TrieNode // Children corresponding to bit 0 and 1
}

// Insert a number into the Trie
func (node *TrieNode) insert(num int) {
    current := node
    for i := 31; i >= 0; i-- { // Assuming 32-bit integer
        bit := (num >> i) & 1
        if current.children[bit] == nil {
            current.children[bit] = &TrieNode{}
        }
        current = current.children[bit]
    }
}

// Find maximum XOR for a number with respect to numbers in the Trie
func (node *TrieNode) findMaximumXOR(num int) int {
    current := node
    maxXOR := 0
    for i := 31; i >= 0; i-- {
        bit := (num >> i) & 1
        // Try to go to the opposite bit for maximizing XOR
        if current.children[1-bit] != nil {
            maxXOR |= (1 << i)
            current = current.children[1-bit]
        } else {
            current = current.children[bit]
        }
    }
    return maxXOR
}

func findMaximumXOR(nums []int) int {
    root := &TrieNode{}
    maxResult := 0

    // Insert all numbers in the Trie
    for _, num := range nums {
        root.insert(num)
    }

    // Find the maximum XOR for each number
    for _, num := range nums {
        currentMax := root.findMaximumXOR(num)
        if currentMax > maxResult {
            maxResult = currentMax
        }
    }
    return maxResult
}

func main() {
    nums := []int{3, 10, 5, 25, 2, 8}
    fmt.Println(findMaximumXOR(nums)) // Output: 28
}
```

### Complexity Analysis
- **Time Complexity**: \(O(n) \times O(\text{bit length})\) - Every insert and lookup is proportional to the bit length (here 32 bits for a standard integer).
- **Space Complexity**: \(O(n \times \text{bit length})\) - Due to space utilized by the Trie, storing parts of each number's binary representation.

