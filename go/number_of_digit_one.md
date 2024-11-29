# 233. [Number of Digit One](https://leetcode.com/problems/number-of-digit-one/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n * log10(n))
// Space Complexity: O(1)
package main

import "fmt"

func countDigitOne(n int) int {
    count := 0

    // Loop through each number from 1 to n
    for i := 1; i <= n; i++ {
        count += countOnesInNumber(i)
    }

    return count
}

// Helper method to count number of 1's in a single number
func countOnesInNumber(num int) int {
    ones := 0
    for num > 0 {
        if num % 10 == 1 {
            ones++
        }
        num /= 10
    }
    return ones
}

func main() {
    fmt.Println(countDigitOne(13)) // Example usage
}
```

## Approach 2: Counting 1s Using Digit Position

### Solution
go
```go
// Time Complexity: O(log10(n))
// Space Complexity: O(1)
package main

import "fmt"

func countDigitOne(n int) int {
    count := 0
    factor := 1 // Factor to isolate digits, start with ones place

    for factor <= n {
        lowerNumbers := n - (n/factor)*factor // Numbers less than current digit place
        currentDigit := (n / factor) % 10     // Current digit

        higherNumbers := n / (factor * 10)    // Numbers higher than current digit place

        if currentDigit == 0 {
            count += higherNumbers * factor // When the current digit is '0'
        } else if currentDigit == 1 {
            count += higherNumbers*factor + lowerNumbers + 1 // When the current digit is '1'
        } else {
            count += (higherNumbers + 1) * factor // When the current digit is '2' or more
        }

        factor *= 10 // Move to next digit position
    }

    return count
}

func main() {
    fmt.Println(countDigitOne(13)) // Example usage
}
```

