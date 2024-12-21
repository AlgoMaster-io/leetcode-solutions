# [Leetcode 357: Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

## Solutions:
- [Brute Force Method](#brute-force-method)
- [Mathematical Counting Approach](#mathematical-counting-approach)

### Brute Force Method
The brute force approach to solving this problem is to generate all numbers with up to `n` digits and count those that have all unique digits. This is feasible for small values of `n` but becomes computationally expensive as `n` increases, due to the large number of combinations.

#### Steps:
1. Initialize a counter to zero.
2. Iterate over each number from 0 to \(10^n - 1\).
3. For each number, check if its digits are unique.
4. If the digits are unique, increment the counter.
5. Return the counter as the result.

The complexity of this method is high due to needing to check each number. However, the implementation helps to understand the problem better.

```go
func countNumbersWithUniqueDigits(n int) int {
    if n == 0 {
        return 1
    }

    count := 0
    upperLimit := int(math.Pow(10, float64(n)))

    // Helper function to check if a number has unique digits.
    hasUniqueDigits := func(num int) bool {
        seen := make(map[int]bool)
        for num > 0 {
            digit := num % 10
            if seen[digit] {
                return false
            }
            seen[digit] = true
            num /= 10
        }
        return true
    }

    // Count all numbers with unique digits
    for i := 0; i < upperLimit; i++ {
        if hasUniqueDigits(i) {
            count++
        }
    }

    return count
}
```

**Time Complexity:** \(O(10^n \times d)\), where `d` is the number of digits in the number.

**Space Complexity:** \(O(1)\).


### Mathematical Counting Approach
A more optimal way to solve this problem uses combinatorial mathematics. When counting unique digit numbers, we focus on the maximum length and calculate possibilities using permutations.

#### Intuition:
1. For a number with unique digits of length `k` (where `1 <= k <= n`), the first digit has 9 possibilities (1 through 9), the second can have 9 (including 0 but excluding the first digit), the third 8, and so on. This continues as \(9 \times 9 \times \ldots\) (up to `k` times).
2. Calculate unique numbers for each digit length from 1 to `n` and sum them up.

```go
func countNumbersWithUniqueDigits(n int) int {
    if n == 0 {
        return 1
    }

    count := 10
    uniqueDigits := 9
    availableNumber := 9

    for i := 2; i <= n && availableNumber > 0; i++ {
        uniqueDigits *= availableNumber
        count += uniqueDigits
        availableNumber--
    }

    return count
}
```

**Time Complexity:** \(O(n)\). We iterate through numbers from 2 to `n` at most.

**Space Complexity:** \(O(1)\). We are using a fixed number of extra variables regardless of `n`.

This optimal solution efficiently uses the concept of permutations to calculate unique numbers with less computation, making it feasible even for larger `n`.

