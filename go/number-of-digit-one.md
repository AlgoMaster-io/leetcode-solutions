# [Leetcode 233: Number of Digit One](https://leetcode.com/problems/number-of-digit-one/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Mathematical Approach / Digit DP](#mathematical-approach--digit-dp)

---

### Brute Force Approach

#### Intuition
The brute force approach involves iterating through each number from 1 to n and counting the number of ones in each number. This method is straightforward but not efficient for large values of n.

#### Steps
1. Initialize a counter to zero.
2. Loop through each number from 1 to n.
3. For each number, convert it to a string (or an integer array) and count the '1's.
4. Add the count to the counter.
5. Return the counter after processing all numbers.

#### Code
```go
func countDigitOneBruteForce(n int) int {
    count := 0
    // Iterate through each number from 1 to n
    for i := 1; i <= n; i++ {
        num := i
        // Count '1's in the current number
        for num > 0 {
            if num%10 == 1 { // Check if the last digit is 1
                count++
            }
            num /= 10 // Remove the last digit
        }
    }
    return count
}
```

#### Time and Space Complexity
- **Time Complexity:** O(n * log(n)), as we iterate through each number and have to check each digit within it.
- **Space Complexity:** O(1), we only use constant extra space for counting.

---

### Mathematical Approach / Digit DP

#### Intuition
The digit dynamic programming method leverages the structure of numbers and their digits to count the number of ones. By focusing on the contribution of each digit position separately, we can efficiently calculate the total count without examining every individual number.

#### Steps
1. Initialize a counter to 0.
2. Use a loop to consider each digit position (units, tens, hundreds, etc.) separately.
3. For each position, calculate three components: lower part, current digit, and upper part.
4. Determine the contribution to the count of ones based on the current digit and its position.
5. Sum the contributions from all positions to get the final result.

#### Code
```go
func countDigitOne(n int) int {
    count := 0
    factor := 1
    currentNumber := n

    // Process each digit position
    for currentNumber > 0 {
        lowerNumbers := n - currentNumber * factor // Lower part
        currentDigit := (currentNumber % 10)       // Current digit
        upperNumbers := currentNumber / 10         // Upper part

        // Contribution from current digit position
        if currentDigit > 1 {
            // If digit > 1, it contributes the upper part + 1 full cycles of 1 occurrence
            count += (upperNumbers + 1) * factor
        } else if currentDigit == 1 {
            // If digit == 1, we need to include all lower numbers + 1 additional occurrence
            count += upperNumbers * factor + (lowerNumbers + 1)
        } else {
            // If digit == 0, only the full cycles of the upper part contribute
            count += upperNumbers * factor
        }

        // Move to the next digit position
        factor *= 10
        currentNumber /= 10
    }

    return count
}
```

#### Time and Space Complexity
- **Time Complexity:** O(log(n)), since we process each digit of the number which is proportional to the log10 of n.
- **Space Complexity:** O(1), we use a constant amount of space.

---

This structured solution explanation should help in understanding the different approaches to solving the "Number of Digit One" problem, from the straightforward brute force method to a more efficient mathematical technique using digit-based processing.

