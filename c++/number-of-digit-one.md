# [Leetcode 233: Number of Digit One](https://leetcode.com/problems/number-of-digit-one/)

## Approaches

- [Brute Force Solution](#brute-force-solution)
- [Mathematical Observation and Counting](#mathematical-observation-and-counting)

---

### Brute Force Solution

#### Intuition

The most straightforward way to solve this problem is to iterate through each number from 1 to \( n \), counting the number of times the digit '1' appears. Although this approach is simple, it isn't optimal for large values of \( n \) but serves as a good starting point for understanding the problem.

#### Approach

1. Initialize a counter to keep track of the total '1's encountered.
2. For each number from 1 to \( n \):
   - Convert the number to a string.
   - For each character in the string, check if it's a '1' and increment the counter if true.
3. Return the counter.

#### Code

```cpp
int countDigitOne(int n) {
    int count = 0;
    // Iterate through each number from 1 to n
    for (int i = 1; i <= n; ++i) {
        int x = i;
        // Check each digit of the number
        while (x > 0) {
            if (x % 10 == 1) { // Check if last digit is 1
                count++;
            }
            x /= 10; // Move to the next digit
        }
    }
    return count;
}
```

#### Time Complexity

- **Time Complexity:** \( O(n \log n) \), as it takes \( O(\log n) \) for each integer to count digits.
- **Space Complexity:** \( O(1) \) as no additional space is used except for variables.

---

### Mathematical Observation and Counting

#### Intuition

Instead of counting '1's digit by digit for every number, a more refined approach involves observing the placement of '1's place-by-place using mathematical reasoning. We'll calculate the number of '1's contributed by each digit (units, tens, hundreds, etc.) by considering all numbers in a particular range.

#### Approach

1. Use a loop to check each digit's place from least significant to most significant.
2. For each place, compute:
   - The prefix number (higher-digit part above the current-digit place).
   - The current digit.
   - The suffix number (lower-digit part below the current-digit place).
3. Based on the current digit, calculate how many '1's contribute from this digit place and add to the result.

#### Code

```cpp
int countDigitOne(int n) {
    if (n <= 0) return 0;
    
    long long count = 0;
    long long factor = 1; // Represents the current digit's place value (units, tens, etc.)
    
    while (n / factor > 0) {
        long long lower = n - (n / factor) * factor; // Lower part of the number
        long long currDigit = (n / factor) % 10;     // Current digit
        long long higher = n / (factor * 10);        // Higher part above current digit
        
        // Calculate number of 1s contributed by current digit
        if (currDigit == 0) {
            count += higher * factor;
        } else if (currDigit == 1) {
            count += higher * factor + (lower + 1);
        } else {
            count += (higher + 1) * factor;
        }
        
        factor *= 10; // Move to next digit place
    }
    
    return count;
}
```

#### Time Complexity

- **Time Complexity:** \( O(\log n) \), since we iterate considering each digit place.
- **Space Complexity:** \( O(1) \) as no additional structures are used.

This approach efficiently calculates the number of '1's by iterating over the number's digits rather than its entire numeric range, leveraging mathematical insights into combination possibilities of upper, current, and lower parts of the number.

