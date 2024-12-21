# [Leetcode 528: Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approaches
- [Approach 1: Prefix Sum and Linear Search](#approach-1-prefix-sum-and-linear-search)
- [Approach 2: Prefix Sum and Binary Search](#approach-2-prefix-sum-and-binary-search)

## Approach 1: Prefix Sum and Linear Search

### Intuition
The idea behind this approach is to utilize a prefix sum array to determine the appropriate interval for weight selection. By maintaining a cumulative sum of the weights, we can ascertain the upper boundaries of each weight segment. When a random number within the total weight range is generated, a linear search allows us to identify which weight interval the random number falls into.

### Steps
1. **Build the Prefix Sum Array**: Create an array where each element at index `i` is the sum of all weights from the start to index `i`.
2. **Generate a Random Number**: Generate a random number between 0 and the sum of all weights.
3. **Linear Search**: Traverse the prefix sum array to find the first index where the prefix sum is greater than the random number, identifying the segment that the number falls into.

### Time Complexity
- **Initialization**: O(n), where n is the number of weights.
- **Pick Index**: O(n) for each pick operation.

### Space Complexity
- O(n), for storing the prefix sum array.

```go
package main

import (
	"math/rand"
)

type Solution struct {
	prefixSum []int
	totalSum  int
}

func Constructor(w []int) Solution {
	prefixSum := make([]int, len(w))
	prefixSum[0] = w[0]

	for i := 1; i < len(w); i++ {
		prefixSum[i] = prefixSum[i-1] + w[i]
	}

	return Solution{prefixSum: prefixSum, totalSum: prefixSum[len(w)-1]}
}

func (s *Solution) PickIndex() int {
	target := rand.Intn(s.totalSum)

	for i, sum := range s.prefixSum {
		if target < sum {
			return i
		}
	}
	return -1 // This should never be reached
}
```

## Approach 2: Prefix Sum and Binary Search

### Intuition
Leveraging binary search can optimize the process of finding the correct index for the random number. By using the prefix sum array as a lookup tool, binary search allows us to find the target interval in logarithmic time, significantly speeding up repeated random selection.

### Steps
1. **Build the Prefix Sum Array**: As in the first approach, create a prefix sum array for cumulative weights.
2. **Generate a Random Number**: Randomly derive a number from 0 to the sum of all weights.
3. **Binary Search**: Use binary search to efficiently find the potential index where the random number aligns with the prefix sum.

### Time Complexity
- **Initialization**: O(n)
- **Pick Index**: O(log n) for each pick operation.

### Space Complexity
- O(n), due to the prefix sum array.

```go
package main

import (
	"math/rand"
	"sort"
)

type Solution struct {
	prefixSum []int
	totalSum  int
}

func Constructor(w []int) Solution {
	prefixSum := make([]int, len(w))
	prefixSum[0] = w[0]

	for i := 1; i < len(w); i++ {
		prefixSum[i] = prefixSum[i-1] + w[i]
	}

	return Solution{prefixSum: prefixSum, totalSum: prefixSum[len(w)-1]}
}

func (s *Solution) PickIndex() int {
	target := rand.Intn(s.totalSum)

	// Using binary search to find the index
	idx := sort.Search(len(s.prefixSum), func(i int) bool {
		return s.prefixSum[i] > target
	})

	return idx
}
```

This binary search approach offers a more efficient mechanism for repeated calls to `PickIndex`, making it more suitable for scenarios requiring frequent weight-based random selections.

