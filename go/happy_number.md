# [202. Happy Number](https://leetcode.com/problems/happy-number/)

## Approach 1: Using HashSet to Detect Cycles

### Solution
```go
// Time Complexity: O(log(n))
// Space Complexity: O(log(n))
package main

import "fmt"

func isHappy(n int) bool {
	seen := make(map[int]struct{})

	for n != 1 && !contains(seen, n) {
		seen[n] = struct{}{}
		n = getNextNumber(n)
	}

	return n == 1
}

func contains(set map[int]struct{}, num int) bool {
	_, exists := set[num]
	return exists
}

func getNextNumber(n int) int {
	sum := 0

	for n > 0 {
		digit := n % 10
		sum += digit * digit
		n /= 10
	}

	return sum
}

func main() {
	fmt.Println(isHappy(19)) // Example usage
}
```

## Approach 2: Fast and Slow Pointers (Optimal for Cycle Detection)

### Solution
```go
// Time Complexity: O(log(n))
// Space Complexity: O(1)
package main

import "fmt"

func isHappy(n int) bool {
	slow := n
	fast := getNextNumber(n)

	for fast != 1 && slow != fast {
		slow = getNextNumber(slow)
		fast = getNextNumber(getNextNumber(fast))
	}

	return fast == 1
}

func getNextNumber(n int) int {
	sum := 0

	for n > 0 {
		digit := n % 10
		sum += digit * digit
		n /= 10
	}

	return sum
}

func main() {
	fmt.Println(isHappy(19)) // Example usage
}
```

