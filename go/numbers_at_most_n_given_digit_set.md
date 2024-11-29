# 902. [Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approach 1: Simple Counting with Recursive Backtracking

### Solution
```go
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D
package main

import "strconv"

type Solution struct{}

func (s *Solution) atMostNGivenDigitSet(digits []string, n int) int {
	nStr := strconv.Itoa(n)
	lenNStr := len(nStr)
	digitArr := make([]rune, len(digits))
	for i, digit := range digits {
		digitArr[i] = rune(digit[0])
	}
	return s.backtrack(digitArr, nStr, 0, true)
}

func (s *Solution) backtrack(digits []rune, nStr string, index int, limit bool) int {
	if index == len(nStr) {
		return 1
	}
	count := 0
	for _, d := range digits {
		if limit && d > rune(nStr[index]) {
			break
		}
		count += s.backtrack(digits, nStr, index+1, limit && d == rune(nStr[index]))
	}
	if !limit {
		factor := 1
		for i := index + 1; i < len(nStr); i++ {
			factor *= len(digits)
		}
		count += factor
	}
	return count
}
```

## Approach 2: Dynamic Programming

### Solution
```go
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D
package main

import "strconv"
import "math"

type Solution struct{}

func (s *Solution) atMostNGivenDigitSet(digits []string, n int) int {
	nStr := strconv.Itoa(n)
	lenNStr := len(nStr)
	dp := make([]int, lenNStr+1)
	dp[lenNStr] = 1

	digitArr := make([]rune, len(digits))
	for i, digit := range digits {
		digitArr[i] = rune(digit[0])
	}

	for i := lenNStr - 1; i >= 0; i-- {
		char := rune(nStr[i])
		for _, d := range digitArr {
			if d < char {
				dp[i] += int(math.Pow(float64(len(digits)), float64(lenNStr-i-1)))
			} else if d == char {
				dp[i] += dp[i+1]
			}
		}
	}

	result := 0
	for i := 1; i < lenNStr; i++ {
		result += int(math.Pow(float64(len(digits)), float64(i)))
	}
	return result + dp[0]
}
```

## Approach 3: Mathematical Counting

### Solution
```go
// Time Complexity: O(n * m)
// Space Complexity: O(1)
// where n is the number of digits in N and m is the number of digits in D
package main

import (
	"strconv"
	"math"
)

type Solution struct{}

func (s *Solution) atMostNGivenDigitSet(digits []string, n int) int {
	nStr := strconv.Itoa(n)
	lenNStr := len(nStr)
	count := make([][2]int, lenNStr)

	power := make([]int, lenNStr+1)
	power[0] = 1
	for i := 1; i <= lenNStr; i++ {
		power[i] = power[i-1] * len(digits)
	}

	for i := 0; i < lenNStr; i++ {
		char := rune(nStr[i])
		for _, digit := range digits {
			d := rune(digit[0])
			if d < char {
				count[i][0] += power[lenNStr-i-1]
			} else if d == char {
				count[i][1] = 1
			}
		}
		if count[i][1] == 0 {
			break
		}
	}

	result := 0
	for i := 0; i < lenNStr; i++ {
		result += count[i][0]
	}

	for i := 1; i < lenNStr; i++ {
		result += power[i]
	}

	result += count[lenNStr-1][1]

	return result
}
```

