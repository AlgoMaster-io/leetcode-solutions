# [Leetcode 233: Number of Digit One](https://leetcode.com/problems/number-of-digit-one/)

## Table of Contents
- [Brute Force Solution](#brute-force-solution)
- [Optimized Math-Based Solution](#optimized-math-based-solution)

### Brute Force Solution

#### Intuition
A straightforward approach is to count the occurrences of digit one ('1') in each number from 1 to n. This involves converting each number to a string and counting the number of '1's in the string representation.

#### Implementation

```csharp
public class Solution {
    public int CountDigitOne(int n) {
        int count = 0;
        
        for (int i = 1; i <= n; i++) {
            // Convert the integer i to string to count '1's
            string number = i.ToString();
            foreach (char c in number) {
                // For each character in the string, check if it's '1'
                if (c == '1') {
                    count++; // Increment count for every '1' found
                }
            }
        }
        
        return count;
    }
}
```

#### Time Complexity
- **O(n * m)**, where `n` is the given number, and `m` is the average number of digits in numbers from 1 to n. The algorithm processes each number up to n and checks each digit.

#### Space Complexity
- **O(1)**, as we do not use any additional data structures that scale with input size, apart from a few variables needed for counting and iteration.

### Optimized Math-Based Solution

#### Intuition
Counting "1" digits occurring on each position (units, tens, hundreds, etc.) separately using mathematical insights can lower the complexity to logarithmic. The key idea is to consider the occurrence of '1' digit at each decimal position (units, tens, hundreds, etc.) and utilize the patterns and frequency of digit one.

1. For each position (units, tens, hundreds), count how many full cycles of 0-9 are there in the numbers from 1 to n.
2. Determine how many times '1' has appeared in units, tens, etc., based on those cycles.

#### Implementation

```csharp
public class Solution {
    public int CountDigitOne(int n) {
        int count = 0;
        long factor = 1;
        
        // Evaluate digit by digit from right to left
        while (factor <= n) {
            // Complete number cycles below the current position
            long lowerNumbers = n - (n / factor) * factor;
            // Rightmost digit at current position
            long currentDigit = (n / factor) % 10;
            // Complete number cycles above the current position
            long higherNumbers = n / (factor * 10);
            
            // Count ones contributed by the higher part and this position
            if (currentDigit == 0) {
                count += higherNumbers * factor;
            } else if (currentDigit == 1) {
                count += higherNumbers * factor + lowerNumbers + 1;
            } else {
                count += (higherNumbers + 1) * factor;
            }
            
            // Move to the next higher position
            factor *= 10;
        }
        
        return count;
    }
}
```

#### Time Complexity
- **O(log(n))**, where `n` is the input number. We iterate over each digit position (units, tens, hundreds, etc.), which is proportional to the logarithm of the number.

#### Space Complexity
- **O(1)**, as we only use a fixed number of variables independent of the input size.

This mathematical approach efficiently calculates the count of '1's up to `n`, significantly improving over the naive method, especially for large `n`.

