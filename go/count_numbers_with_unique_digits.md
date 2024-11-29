# 357. [Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

## Approach 1: Recursive Backtracking

### Solution
go
```go
// Time Complexity: O(10^n)
// Space Complexity: O(n)
package main

func countNumbersWithUniqueDigits(n int) int {
    if n == 0 {
        return 1
    }
    used := make([]bool, 10)
    return countNumbers(0, n, used)
}

func countNumbers(current, n int, used []bool) int {
    if n == 0 {
        return 1
    }
    count := 1 // count the number itself
    for i := 0; i < 10; i++ {
        if !used[i] {
            used[i] = true
            count += countNumbers(current*10+i, n-1, used)
            used[i] = false
        }
    }
    return count
}
```

## Approach 2: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func countNumbersWithUniqueDigits(n int) int {
    if n == 0 {
        return 1
    }

    uniqueNumbers := 10
    availableDigits := 9
    currentUnique := 9

    // Use dynamic programming to calculate the number
    for i := 2; i <= n; i++ {
        currentUnique *= availableDigits
        uniqueNumbers += currentUnique
        availableDigits--
    }

    return uniqueNumbers
}
```

## Approach 3: Formula-based Calculation

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func countNumbersWithUniqueDigits(n int) int {
    if n == 0 {
        return 1
    }

    count := 10 // start counting from single-digit numbers
    uniqueDigits := 9
    availableNumbers := 9

    // Calculate using the permutation formula for unique digits
    for n > 1 && availableNumbers > 0 {
        uniqueDigits *= availableNumbers
        count += uniqueDigits
        availableNumbers--
        n--
    }

    return count
}
```

