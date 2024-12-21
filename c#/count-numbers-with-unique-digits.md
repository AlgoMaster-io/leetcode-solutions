# [Leetcode 357: Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

## Approaches

- [Brute Force Approach](#brute-force-approach)
- [Combinatorial Approach](#combinatorial-approach)

### Brute Force Approach

#### Intuition

In the brute force approach, we will check each number from 0 to \(10^n - 1\) and count how many numbers have all unique digits. This approach, albeit straightforward, is not efficient due to the high computational cost for large values of \(n\).

#### Implementation

We can implement this approach by iterating through every number and eliminating those numbers which have repeating digits:

```csharp
public class Solution {
    public int CountNumbersWithUniqueDigits(int n) {
        if (n == 0) return 1;
        
        int limit = (int)Math.Pow(10, n);
        int count = 0;
        
        for (int i = 0; i < limit; i++) {
            if (HasUniqueDigits(i)) {
                count++;
            }
        }
        
        return count;
    }
    
    private bool HasUniqueDigits(int num) {
        bool[] digitSeen = new bool[10];  // Array to track if a digit has been seen
        while (num > 0) {
            int digit = num % 10;           // Extract the last digit
            if (digitSeen[digit]) return false; // Check if the digit has been seen before
            digitSeen[digit] = true; // Mark the digit as seen
            num /= 10;
        }
        return true; // All digits are unique
    }
}
```

#### Time and Space Complexity

- **Time Complexity**: \(O(10^n \times d)\) where \(d\) is the average number of digits, since we iterate through \(10^n\) numbers and check each number's digits.
- **Space Complexity**: \(O(1)\), apart from the integer storage and the fixed-size array.

### Combinatorial Approach

#### Intuition

The combinatorial approach takes advantage of the unique properties of numbers and combinations. For each digit, we consider how many options are available without repeating any digits. We start from smaller numbers and work our way up by dynamically multiplying the possibilities.

- For n = 1: There are 10 numbers (0 to 9).
- For n = 2: The first digit has 9 options (1 to 9), and the second digit has 9 options (0 to 9 without the chosen first digit).
- This pattern continues for n = 3, with the third digit having 8 options, and so on.

The number of unique digit numbers can be calculated using multiplicative counting.

#### Implementation

The implementation uses dynamic programming to accumulate the results:

```csharp
public class Solution {
    public int CountNumbersWithUniqueDigits(int n) {
        if (n == 0) return 1;
        
        int count = 10; // For n = 1
        
        // Start calculating the number of unique digit numbers for higher n
        int uniqueDigits = 9; // Initial available digits for the first position 
        int availableNumber = 9; // Remaining digits for subsequent positions
        
        for (int i = 2; i <= n; i++) {
            uniqueDigits *= availableNumber;
            count += uniqueDigits;
            availableNumber--;
        }
        
        return count;
    }
}
```

#### Time and Space Complexity

- **Time Complexity**: \(O(n)\), since we iterate through digits up to \(n\).
- **Space Complexity**: \(O(1)\), since only a few integer variables are used regardless of \(n\).

This combinatorial approach is more efficient and can handle larger values of \(n\) efficiently compared to the brute-force method.

