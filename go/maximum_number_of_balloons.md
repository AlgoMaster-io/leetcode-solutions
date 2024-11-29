# [1189. Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approach 1: Frequency Count with HashMap

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

import "fmt"

func maxNumberOfBalloons(text string) int {
	countMap := make(map[rune]int)

	// Count frequencies of each character in the text
	for _, c := range text {
		countMap[c]++
	}

	// Check the minimum occurrences of characters required for "balloon"
	b := countMap['b']
	a := countMap['a']
	l := countMap['l'] / 2 // 'l' appears twice in "balloon"
	o := countMap['o'] / 2 // 'o' appears twice in "balloon"
	n := countMap['n']

	// Return the minimum number of complete "balloon" words
	return min(min(min(b, a), l), min(o, n))
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func main() {
	fmt.Println(maxNumberOfBalloons("loonbalxballpoon")) // Output: 2
}
```

## Approach 2: Frequency Count with Array

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

import "fmt"

func maxNumberOfBalloons(text string) int {
	charCount := [26]int{} // Frequency array for all characters

	// Count frequencies of each character in the text
	for _, c := range text {
		charCount[c-'a']++
	}

	// Check the minimum occurrences of characters required for "balloon"
	b := charCount['b'-'a']
	a := charCount['a'-'a']
	l := charCount['l'-'a'] / 2 // 'l' appears twice in "balloon"
	o := charCount['o'-'a'] / 2 // 'o' appears twice in "balloon"
	n := charCount['n'-'a']

	// Return the minimum number of complete "balloon" words
	return min(min(min(b, a), l), min(o, n))
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func main() {
	fmt.Println(maxNumberOfBalloons("loonbalxballpoon")) // Output: 2
}
```

## Approach 3: Optimized Single Pass

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

import "fmt"

func maxNumberOfBalloons(text string) int {
	counts := [5]int{} // Indices: 0->b, 1->a, 2->l, 3->o, 4->n

	// Count only relevant characters
	for _, c := range text {
		switch c {
		case 'b':
			counts[0]++
		case 'a':
			counts[1]++
		case 'l':
			counts[2]++
		case 'o':
			counts[3]++
		case 'n':
			counts[4]++
		}
	}

	// 'l' and 'o' need to be divided by 2 as they appear twice in "balloon"
	counts[2] /= 2
	counts[3] /= 2

	// Return the minimum count
	min := counts[0]
	for _, count := range counts {
		if count < min {
			min = count
		}
	}

	return min
}

func main() {
	fmt.Println(maxNumberOfBalloons("loonbalxballpoon")) // Output: 2
}
```

